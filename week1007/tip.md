## ARTS - Tip

在启动项目时候，由于本地安装了mysql8, 所以有报错：

```
Caused by: java.sql.SQLException: Unknown system variable 'tx_isolation'
```
看数据库版本：

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 124
Server version: 8.0.11 Homebrew

```

看是否有tx_isolotion

```
mysql> show variables like 'tx_isolation';
Empty set (0.00 sec)

```

因为在mysql8里把这个变量改成了 'transaction_isolation'

```
mysql> show variables like 'transaction_isolation';
+-----------------------+-----------------+
| Variable_name         | Value           |
+-----------------------+-----------------+
| transaction_isolation | REPEATABLE-READ |
+-----------------------+-----------------+
1 row in set (0.01 sec)
```



我们再看下mysql7的变量

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1185395
Server version: 5.7.20 MySQL Community Server (GPL)



mysql> show variables like 'tx_isolation'
    -> ;
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| tx_isolation  | REPEATABLE-READ |
+---------------+-----------------+
1 row in set (0.00 sec)


mysql> show variables like 'transaction_isolation';
+-----------------------+-----------------+
| Variable_name         | Value           |
+-----------------------+-----------------+
| transaction_isolation | REPEATABLE-READ |
+-----------------------+-----------------+
1 row in set (0.00 sec)

```
居然两个都有，但是在mysql8里就只有一个了，所以要想使用mysql8， 就需要把驱动更新到mysql8, 


官方文档提提到了这个变动：
https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-3.html


> The deprecated tx_isolation and tx_read_only system variables have been 
> removed. Use **transaction_isolation** and **transaction_read_only** instead.

