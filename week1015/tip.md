## ARTS - Tip
## MySQL 大分页问题

### 问题
我们知道在进行MySQL分页时候最常用 limit offset, size, 但是当数据量很大，达到千万级别，直接分页会很慢，所以需要进行优化。

### 验证
我们进行测试：


```
mysql> select count(*) from table ;
+----------+
| count(*) |
+----------+
| 16079918 |
+----------+
1 row in set (2.38 sec)

```

表里有一千六百万挑数据， 我们分别进行 千、十万、百万、千万 级别进行 测试

```
mysql> select id, user_name, mobile from table limit 1000, 10;
---
10 rows in set (0.00 sec)


mysql> select id, user_name, mobile from table limit 100000, 10;
---
10 rows in set (0.08 sec)


mysql> select id, user_name, mobile from table limit 1000000, 10;
---
10 rows in set (0.67 sec)


mysql> select id, user_name, mobile from table limit 10000000, 10;
---
10 rows in set (7.04 sec)

```

发现在百万以下，反应速度还可以忍受，上了千万，耗时7秒多，这已经很影响性能了，所以我们要进行优化。

### 优化方案

尽量在大数量查询时候使用索引，先用覆盖索引查出ID，在进行下一步查找。

于是SQL改进为：

```
mysql> select  user_name, mobile from table where id >= (select id from table limit 10000000, 1) limit 10;
---
10 rows in set (1.53 sec)
```

也可以用内连接 

```
mysql> select  user_name, mobile from table a  join (select id from table limit 10000000, 10) b on a.id = b.id;
---
10 rows in set (1.64 sec)
```

这样把7秒的查询下降到2秒内，还是可以接受的。
