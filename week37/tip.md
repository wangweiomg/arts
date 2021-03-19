## ARTS - Tip 补2019.3.20

## 一个关于MongoDB连接的问题

最近有一个需求，要连接mongo集群，但是总是报错，以下是问题及解决过程。

### 项目背景

springboot项目，连接阿里云MongoDB数据库。

### 问题描述

在启动项目时候报错如下：

```java
No server chosen by ReadPreferenceServerSelector{readPreference=primary} from cluster description ClusterDescription{type=UNKNOWN, connectionMode=MULTIPLE, serverDescriptions=[ServerDescription{address=xxx:3717, type=UNKNOWN, state=CONNECTING, excepti
on={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}, ServerDescription{address=xxx:3717, type=UNKNOWN, state=CONNECTING, excep
tion={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}]}. Waiting for 30000 ms before timing out
[2020-10-22 09:09:01.844][ERROR][unknown_8ea74c3c095b4c689a1b989f25fc6d9f][c.o.o.c.c.CRBDataQueryController]- userMobileList error 2348162338435 null 499611240792850432
org.springframework.dao.DataAccessResourceFailureException: Timed out after 30000 ms while waiting for a server that matches ReadPreferenceServerSelector{readPreference=primary}. Client view of cluster state is {type=UNKNOWN, servers=[{address=xxx:3717, type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}, {address=xxx:3717, type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}]; nested exception is com.mongodb.MongoTimeoutException: Timed out after 30000 ms while waiting for a server that matches ReadPreferenceServerSelector{readPreference=primary}. Client view of cluster state is {type=UNKNOWN, servers=[{address=xxx:3717, type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}, {address=xxx:3717, type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}]
```

### 原因分析

具体报错是：

```java
Caused by: com.mongodb.MongoTimeoutException: Timed out after 30000 ms while waiting for a server that matches ReadPreferenceServerSelector{readPreference=primary}.

```

看到MongoTimeoutException， 就是MongoDB连接不上，因为以前也碰到过MongoDB报错问题，大部分情况是driver版本问题，就以为也是版本问题。但是这次有具体的报错信息， **Prematurely reached end of stream**  就拿着这个去搜索，发现各种回答主要有一下四种：

1. timeout问题。MongoDB默认空闲时间是0，需要设置 timeout maxIdleTimeMS=3000， 来避免客户端不知服务端已关闭连接
2. Mongo java驱动版本问题。如果版本不匹配，很容易造成连接失败
3. 用户名、密码、地址端口问题
4. 防火墙问题

### 验证过程

由于最初怀疑MongoDB 驱动版本问题，就去MongoDB官网查找驱动对照表，发现版本正常。排除2. 

然后最不能就是用户名、密码问题，因为都是照着发布信息复制的，不会填错，不过也确实再次比对，发现不是，排除3.

然后是1， 由于要改代码配置，就最后来试，改完发布后发现问题依然没解决，这时候怀疑是不是整体配置问题？还是改的配置没生效。然后继续查官方文档，找对应的配置方法，检查代码整体配置，发现没有问题。不太情愿排除1.

最后查防火墙。一般来说，系统服务器资源被开出来，就已经代表运维同学完成了各项基本配合、连接问题，之前几次项目都是这样。不过这次就要再检查下，使用netstat 尝试IP和端口， **发现是不通的!** 于是找来运维同学确认问题，发现确实没有加白名单，之后问题就顺利解决了。

### 结论

对于问题分析，经验主义确实重要，让你更快确定问题范围，但不要只信经验主义，被先入为主的观念束缚，就很容忽略显而易见的事实。没有不会出错的经验，可以依赖的只有事实和逻辑。