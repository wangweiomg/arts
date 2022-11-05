## ARTS - Tip  补2019.3.6

## MySQL的 ON DUPLICATE KEY UPDATE

### 背景

项目中经常使用到saveOrUpdate方式更新，即没有就插入，有就更新。发现MySQL支持这样的语法，[13.2.6.2 INSERT ... ON DUPLICATE KEY UPDATE Syntax](https://dev.mysql.com/doc/refman/8.0/en/insert-on-duplicate.html)

### 官方文档

> If you specify an `ON DUPLICATE KEY UPDATE` clause and a row to be inserted would cause a duplicate value in a `UNIQUE` index or `PRIMARY KEY`, an [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html) of the old row occurs. For example, if column `a` is declared as `UNIQUE` and contains the value `1`, the following two statements have similar effect:

如果你指定了 ON DUPLICATE KEY  UPDATE  语句，插入的行将会造成一个 UNIQUE 索引或PRIMARY KEY 的列的值重复，将会UPDATE 旧的值。例如，如果列  a  声明为unique并且包含值为 1， 一下两个语句有同样的效果：

```sql
insert into t1(a, b, c) values(1, 2, 3) ON DUPLICATE KEY UPDATE C=C+1;

update t1 set c=c+1 where a=1;
```

(如果一个 InnoDB 表的 a 列指定为自增，表现会不同， INSERT语句会增加自增值，但update不会)

如果一个列 b  也是 unique, 该insert 和如下UPDATE语句相同：

```sql
UPDATE t1 SET c=c+1 WHERE a=1 OR b=2 LIMIT 1;
```

If a=1 或 b=2 匹配多行，仅仅一行被更新。通常，你应该在该表有多个 unique 索引时避免使用 ON DUPLICATE KEY UPDATE 语句。

使用 ON DUPLICATE KEY UPDATE 时，受影响的行数， 如果是新插入一行，就是1；如果存在就更新，就是2，如果存在行被设置为本来的值，就是0.

如果一个表包含一个 AUTO_INCREMENT 列， INSERT... ON DUPLICATE KEY UPDATE  插入或更新一行，LAST_INSERT_ID() 返回的是AUTO_INCREMENT的值。



ON DUPLICATE KEY UPDATE 语句可以包含多个列，逗号分隔。

你可以使用VALUES(col_name) 函数，指的是将会被插入的 col_name 列。

```sql
INSERT INTO t1 (a,b,c) VALUES (1,2,3),(4,5,6)
  ON DUPLICATE KEY UPDATE c=VALUES(a)+VALUES(b);
```

和以下相同：

```sql
INSERT INTO t1 (a,b,c) VALUES (1,2,3)
  ON DUPLICATE KEY UPDATE c=3;
INSERT INTO t1 (a,b,c) VALUES (4,5,6)
  ON DUPLICATE KEY UPDATE c=9;
```

对于 INSERT ... SELECT 语句，这些规则适用于正常的SELECT语句，可以参考ON DUPLICATE KEY UPDATE :

* 从一个可能是派生的单表查的列
* 多表连接查询的列
* DISTINCT查的列
* 不用GROUP BY 查询的列，一个副作用是你必须给非唯一列名一个别名

对于UNION的列不支持，但是可以作为一个派生表，然后被当做单表结果，如：

```sql
INSERT INTO t1 (a, b)
  SELECT c, d FROM t2
  UNION
  SELECT e, f FROM t3
ON DUPLICATE KEY UPDATE b = b + c;
```

修改为：

```sql
INSERT INTO t1 (a, b)
SELECT * FROM
  (SELECT c, d FROM t2
   UNION
   SELECT e, f FROM t3) AS dt
ON DUPLICATE KEY UPDATE b = b + c;
```

### 实际使用

我们在往表里查数据时候，使用到了如下结构

```sql
INSERT INTO t1(a, b, c)
	SELECT d,e,f FROM t2
ON DUPLICATE KEY UPDATE a=values(a), b=VALUES(b), c=VALUES(c)
```



这里特别说下 VALUES 函数， t1表中存在数据：



```java
a | b |  c  
-|c|-
1 | 2 | 3 |
4 | 5 | 6 |
```

其中 b 是unique 约束,我们想要更新多个值

```sql

insert into t1 (b, c)
    values(2, 333),(5, 666)
on duplicate key update
  b=22,c=44;
```

这么插入会报错 ```[23000][1062] Duplicate entry '22' for key 'b'```，因为插入多行，会默认插入多行重复的22, 这种情况要用values,改写为：

```sql
insert into t1 (b, c)
    values(2, 333),(5, 666)
on duplicate key update
  b=values(b),c=values(c);
```

这样会正常插入，但是我们如果不用vaues, 写成如下：

```sql
insert into t1 (b, c)
    values(2, 333),(5, 666)
on duplicate key update
  b=b,c=c;
```

不会报错，但是值却没有改变，不是想要的更新。