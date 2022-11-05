## ARTS - Tip 补12.24
## MySQL报错 Packet for query is too large


### 问题出现

今日碰到了一个mysql报错如下：

```
com.mysql.jdbc.PacketTooBigException: Packet for query is too large (4281662 > 4194304). You can change this value on the server by setting the max_allowed_packet' variable.

```

这句话说查询包太大了，这里有一个参数叫 ```max_allowed_packet```，查询资料发现是MySQL设置的最大查询发送包大下，现在看来是超过了。查找相应的SQL语句，确实传送了大量的数据作为参数，然后就优化了SQL，避开这种使用方式。

### max_allowed_packet

这个参数是什么呢?

查询MySQL文档，发现这个是系统变量，在MySQL版本<=8.0.2 时候默认是4194304，也就是4M，大于这个版本是默认64M.

> The maximum size of one packet or any generated/intermediate string, or any parameter sent by the mysql_stmt_send_long_data() C API function. The default is 64MB.


> The packet message buffer is initialized to net_buffer_length bytes, but can grow up to max_allowed_packet bytes when needed. This value by default is small, to catch large (possibly incorrect) packets.
> 

这里提到了一个 net_buffer_length , 这个参数默认是 16384, 也就是16K. 在不够用时候会增长到max_allowed_packet 大小。

> You must increase this value if you are using large BLOB columns or long strings. It should be as big as the largest BLOB you want to use. The protocol limit for max_allowed_packet is 1GB. The value should be a multiple of 1024; nonmultiples are rounded down to the nearest multiple.

你在使用大的 BLOB列或长的strings 时候，应该变大，为1024的倍数，最大为1G。


> When you change the message buffer size by changing the value of the max_allowed_packet variable, you should also change the buffer size on the client side if your client program permits it. The default max_allowed_packet value built in to the client library is 1GB, but individual client programs might override this. For example, mysql and mysqldump have defaults of 16MB and 24MB, respectively. They also enable you to change the client-side value by setting max_allowed_packet on the command line or in an option file.

当在服务端改变这个大小时候，也要在客户端跟着改变。


> The session value of this variable is read only. The client can receive up to as many bytes as the session value. However, the server will not send to the client more bytes than the current global max_allowed_packet value. (The global value could be less than the session value if the global value is changed after the client connects.)
> 

客户端连接后如果全局变量被修改，那么实际的服务端传输大小会小于客户端设置的大小。


