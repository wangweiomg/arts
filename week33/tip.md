## ARTS - Tip 补2019.2.20

MySQL 的GTID

#### 背景

在数据库备份与恢复中碰到了不能导入的情况，报错是GTID相关，查了资料，发现是MySQL在5.6时引入的。

#### 概念

GTID (Global Transaction Identifier) 即全局I事务ID，GTID实际是由UUID 和 TID 组成的。UUID是MySQL实例的唯一标识。TID代表了该实例上已经提交的事务数量，并且随着事务提交单调递增，所以GTID能保证每个MySQL实例事务的执行(不会重复执行同一个事务，并且会补全没有执行的事务)。具体的看官方文档：[17.1.3 Replication with Global Transaction Identifiers](https://dev.mysql.com/doc/refman/8.0/en/replication-gtids.html)

#### GTID 意义

引入GTID的意义是什么?

1. 因为清楚了GTID的格式，所以通过UUID可以知道这个事务是在哪个实例上提交的。
2. 通过GTID可以方便进行复制结构上的故障转移，新主设置。