## ARTS - Tip 补2019.3.13



### MySQL 的CAST转换函数

最近在做一个需求需要连接查询， 如下

```sql
SELECT a.column1, a.column2, b.column1
FROM table1 a 
LEFT JOIN table b on a.id = b.id
WHERE 
...
```

由于 a.id 和 b.id 类型不一致，所以需要转换下,那么，比如

```sql
a.id = CAST(b.id AS CHAR(20))
```

如果要走索引比较条件左边是不能带函数的

```sql
-- 反例
SELECT 
	*
FROM table1
WHERE
DATE_FORMAT(a.create_time, '%Y-%m-%d') = '2020-10-21';

-- 正例
SELECT 
	*
FROM table1
WHERE
a.create_time = STR_TO_DATE('2020-10-21', '%Y-%m-%d');
```

如果要左边写函数，那么就要加**函数索引**。



| Name      | Desciption                     |
| --------- | ------------------------------ |
| BINARY    | string转为二进制string         |
| CAST()    | Cast a value as a certain type |
| CONVERT() | Cast a value as a certain type |

CAST 函数可以改变数据类型



CONVERT()使用USING语句来改变字符集

```sql
CONVERT(expr USING transcoding_name)
```







