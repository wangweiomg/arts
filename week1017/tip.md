## ARTS - Tip
## 谈谈MySQL慢查询

### 什么是慢查询
MySQL的慢查询是MySQL提供的一种记录查询慢的SQL的工具。方便做系统优化。


### 开启慢查询日志
默认情况下是关闭的，

```

mysql> show variables like 'slow_query%';
+---------------------+-------------------------------------+
| Variable_name       | Value                               |
+---------------------+-------------------------------------+
| slow_query_log      | OFF                                 |
| slow_query_log_file | /usr/local/var/mysql/bogon-slow.log |
+---------------------+-------------------------------------+
2 rows in set (0.01 sec)


ysql> show variables like 'long_query_time';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set (0.00 sec)

```


设置方法

全局变量设置

```
#将 slow_query_log 全局变量设置为“ON”状态

mysql> set global slow_query_log='ON'; 

#设置慢查询日志存放的位置

mysql> set global slow_query_log_file='/usr/local/mysql/data/slow.log';

#查询超过1秒就记录

mysql> set global long_query_time=1;
```


### explain 分析

设置慢查询日志后，就会在日志里记录慢查询SQL， 然后使用 explain 分析。

```
mysql> explain select * from Employee where AGE > 30;
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | Employee | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    5 |    33.33 | Using where |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------------+

```





列名|说明|
---|---|
id| 执行编号，标识select所属的行。如果在语句中没子查询或关联查询，只有唯一的select，每行都将显示1。否则，内层的select语句一般会顺序编号，对应于其在原始语句中的位置
select_type|显示本行是简单或复杂select。如果查询有任何复杂的子查询，则最外层标记为PRIMARY（DERIVED、UNION、UNION RESUlT）
table|访问引用哪个表（引用某个查询，如“derived3”）
type|数据访问/读取操作类型（ALL、index、range、ref、eq_ref、const/system、NULL）
possible_keys|揭示哪一些索引可能有利于高效的查找
key|显示mysql决定采用哪个索引来优化查询
key_len|显示mysql在索引里使用的字节数
ref|显示了之前的表在key列记录的索引中查找值所用的列或常量
rows|为了找到所需的行而需要读取的行数，估算值，不精确。通过把所有rows列值相乘，可粗略估算整个查询会检查的行数
Extra|额外信息，如using index、filesort等


