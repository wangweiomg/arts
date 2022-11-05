## ARTS - Share
## 关于日志那些事

日志有访问日志，行为日志……不同类型的日志有不同的格式，另外，还有日志的滚动归档，还有像systemd/syslog等这样的系统日志系统……

### 什么是日志

计算机中，用来记录操作系统或其他软件运行事件，或不同用户操作行为的文件。日志是储存在文件里的动作。在最简单的例子中，信息被写入一个单个文件中。

一个事务日志就是一个系统和系统用户之间交流记录的文件，或者一个数据采集方法，用于自动捕获具有该系统终端操作事务的人员的类型，内容或者时间。网页服务日志文件，但是任何日志文件都可以使用科学日志分析工具来进行分析。

很多操作系统，软件框架和程序都包含一个日志系统。一个广泛使用的日志标准是 syslog. IETF RFC规范的 .

#### syslog 规范
syslog 在计算机中是一个记录日志的规范，它根据软件类型和日志级别生成信息，使用一些助记代码。

计算机系统设计者可能会使用syslog来管理系统和安全验证，也会进行通用信息，分析，排查问题。

日志级别

|值|级别|关键字|描述|
|---|---|---|---|
0| Emergency| emerg|系统不可用
1|Alert|alert|需要尽快恢复的状态，例如系统数据库错误
2|Critical|crit|硬件错误
...





### 日志类型
日志文件记录着当前程序执行的情况，或者管理员的活动。日志对进行一次错误排查是有帮助的。

按日志产生的地方有三种类型的日志：

1. 访问日志， 这里记录当前请求执行结果的日志，每次实际请求都会记录。
2. 行为日志， 这里记录当前管理人员行为的日志，会列出这个人员的所有请求操作。
3. 内部并发管理日志，记录内部并发系统的参数值。

操作系统的日志

sytemd 和 syslogd

linux日志系统是系统 daemon或syslogd. 当系统运行级别为1时候daemon随系统启动。一旦启动，几乎系统的任何部分，包括应用、驱动、和其他守护程序都会产生日志。

在现代Linux发行版syslogd被更新的syslog规范实现例如rsyslog、syslog-ng取代。
这三个不同点是，syslog工程是首个系统日志工程，1980年就建立了，它是syslog协议的基础。同时Syslog是非常简单的协议。开始只支持UDP传输，所以它不能保证可靠传输。

下一个日志系统是 syslog-ng 在1998年诞生。它继承了基础syslog协议并增加了新特性例如：

* 基于内容的过滤器
* 直接把日志存入数据库
* TCP传输
* TLS加密


下一个日志系统是RsysLog 在2004年。继承了syslog协议,增加了新特性比如：

* RELP协议支持
* 缓冲操作支持








### 日志归档

* 滚动策略， 设置最多归档文件数
* 控制触发策略 根据最大文件数触发
* maxHistory策略， 使用maxHistory保留天数设置，超过时间的删除。


--- 

[1]What is the difference between syslog, rsyslog and syslog-ng?  https://serverfault.com/questions/692309/what-is-the-difference-between-syslog-rsyslog-and-syslog-ng

[2]syslog https://en.wikipedia.org/wiki/Syslog
[2]Log file https://en.wikipedia.org/wiki/Log_file
