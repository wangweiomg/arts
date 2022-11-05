## ARTS - Tip 补12.3
### MySQL备份数据过滤与自动化

#### 导数据
在工作中要同步生产数据到测试服务器的需求，以方便测试，于是经常要复制表.

最开始我是这么做的：

```
// 首先备份表
mysqldump -uusername  -p dbname tablename > tablename.sql
// 把表复制到其他服务器
scp tablename.sql user@ip:~/

// 登录user机器，导入数据
mysql -uusername -p dbname < tablename.sql
```

由于测试机测试和本机测试还是不一样，有时使用  sz  tablename.sql 到本地，再测试，这种情况下如果网络不好，而且数据文件非常大，要很慢才能传送完毕，于是就想到，有些数据按天产生，只需要最近几天的数据，对于之前的没必要传送。于是查文档找到了方案.

#### 备份数据过滤

> 
> --tables
> Override the --databases or -B option. mysqldump regards all name arguments  following the option as table names.
>
> 
>  
> --where='where_condition', -w 'where_condition'

>Dump only rows selected by the given WHERE condition. Quotes around the condition are mandatory if it contains spaces or other characters that are special to your command interpreter.

>Examples:

> --where="user='jimf'"
> 
> -w"userid>1"

> -w"userid<1"
> 
> 
> 

可以使用 --tables 和 --where 来过滤， 
如我有t_user表

```
mysql> select * from t_user;
+----+------+------+-------------+
| id | name | age  | telephone   |
+----+------+------+-------------+
|  1 | jack |   20 | 18877771111 |
|  2 | rose |   33 | 18977771111 |
|  3 | tom  |   21 | 13877761111 |
|  4 | bob  |   24 | 13356771111 |
+----+------+------+-------------+
```
要备份id >2的数据，使用

```
mysqldump -uroot -p dbname --tables t_user --where="id>2" > t_user.sql

```
打开文件t_user.sql， 部分如下

```
--
 -- WHERE:  id>2

 LOCK TABLES `t_user` WRITE;
 /*!40000 ALTER TABLE `t_user` DISABLE KEYS */;
 INSERT INTO `t_user` VALUES (3,'tom',21,'13877761111'),(4,'bob',24,'13356771111');
 /*!40000 ALTER TABLE `t_user` ENABLE KEYS */;
 UNLOCK TABLES;
```
可见，已经生效了


#### 脚本化
我想自动化同步表数据，于是就写了个shell脚本来实现，
对于同步服务器脚本如下：

```
#!/bin/bash
 echo "####start sync data to test####"

 for i in "$@"; do
     echo $i
     mysqldump -uusername -ppassword db $i > "$i.sql"
     scp "$i.sql" user@test_ip:~/script/ &
 done

 wait
 rm ./*.sql
 echo "####end sync data####"
```

该脚本参数是要同步的表，对于测试服务器脚本如下：

```
#!/bin/bash
 echo "####start import sql####"

 for i in $(ls *.sql); do
     echo $i
     mysql -uusername -ppassword db <  "$i" &
 done

wait
mv *.sql ../tmp

 echo "### end import###"
```

以上就做到了简单了数据同步，还没实现在服务器A调用B的脚本的功能，以后在加。
