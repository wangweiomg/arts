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

假设一主两从服务器， Server1 主， Server2 从，Server3 从，Server1崩溃，我们要提升Server2、Server3其中之一为主，提升的前提是跟上了主，我们发下Server2跟上了主，Server3没有跟上，这时候就提升Server2为主，然后让Server3跟到和Server2一样的位置，跟上的操作其实就是把Server2已经执行或，但是Server2没执行过的事务再走一遍，这个过程就需要定位事务了，在GTID出现之前是比较难定位的，出现了实例 UUID + 事务数量TID组成的GTID之后，事情就容易了，很容易补全剩下的事务。



#### 总结

GTID， 主从结构只有一台Master和一台Slave，对于GTID来说就没有优势了，对于2台以上的结构优势明显，可以在数据不丢失的情况下切换新主。

使用GTID要注意，构建主从复制之前，在一台将成为主的实例上进行一些操作(如数据清理等)，通过GTID复制，这些在主从成立之前的操作也会复制到从服务器上，因为复制失败。

即：通过GTID复制都是从最先开始的事务日志开始，即使这些操作在复制之前执行。

比如是server1上执行drop\delete清理工作，接着在server2上执行change操作，会使得server2进行server1的清理工作。



---

文章引用：

[[MySQL5.6 新特性之GTID](https://www.cnblogs.com/zhoujinyi/p/4717951.html)]