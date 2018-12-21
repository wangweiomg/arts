## ARTS - Share 补11.19
## Linux 进程浅析
### 什么是进程
进程是在操作系统里开展工作。一个程序就是机器码命令和存储在硬盘可执行镜像上的数据的集合，就是一个被动实体；一个进程可以被看做一个运行中的计算机程序。

它是一个动态实体，随着机器码指令被处理器执行不断变化着。和程序的命令与数据一样，进程也包含程序计数器和所有的CPU 的机器村 也有进程栈包含像常规参数这样的临时数据，返回地址和保存变量。

Linux是多任务操作系统，进程之间互不影响，各自有各自的工作空间独立工作，除非安全问题和内核管理机制影响。

### Linux 进程
Linux进程用 task_struct 的数据结构表示，使用task vector 集合来保存 task_struct 的指针。task vector 的最大长度就是系统支持的最大进程数，默认是512个。

Linux不仅支持普通类型的进程，还是支持实时进程。这些进程需要实时响应事件，所以被调度器区别对待。

Linux的 task_struct 的属性被分成几个方法区：

#### 状态
1. 运行态：进程要么是当前进程正在运行，要么是已经准备好等待获得CPU执行 
2. 等待： 进程等待获取某事件或资源。等待态的进程有两种类型： 可中断 和不可中断进程
3. 停止： 进程停止。通常是接收到了一个停止信号。进程被 debug时也可以进入停止态。
4. 僵尸态：暂停的进程，由于种种原因还存在于task vector中，就像死进程一样。

#### 调度信息
调度器需要了解这些信息以便公正的作出决定下一个运行哪个进程。

#### 标识符
每个进程在系统里都有标识符。进程标识符不是task vector的索引，他就是一个简单数字。每个进程也有用户和组的标识符，这被用来控制进程操作系统里的文件和设备。

#### 进程间通信
Linux支持经典Unix IPC的信号、管道、信号量机制，也支持系统 V IPC的共享内存，信号量，消息队列机制。

#### 关联
在Linux中没有进程是完全独立其他进程的。每个系统中的进程初始化后都有一个父进程。新进程不是被创建的，是被复制的，或者干脆克隆之前的进程。每个 task_struct 代表的进程都指向它的父进程、兄弟进程和子进程。

#### 时间和定时器
内核持续追踪一个进程在其生命周期的创建时间和消费的CPU时间。

#### 文件系统
进程可以打开关闭他们需要的文件、task_struct包含的指向的打开的文件，还有指向两个VFS inodes的文件。


#### 虚拟内存
大多数进程有虚拟内存（内核线程和守护进程没有），Linux内核必须追踪虚拟内存是如何映射到系统物理内存上的。

#### 进程独特上下文
一个进程可以看做系统当前状态的总和。当一个进程运行时它使用处理器的寄存器，栈等等。这就是的进程的上下文，当一个进程暂停，所有关于CPU的独特上下文必须保存进task_struct，这样被调度器重新启动时候能在这个点上恢复。

### 进程的标识符-进程号
系统启动的第一个进程是init, PID 是1，唯一一个由系统内核直接运行的进程。除了init之外每个进程都有父进程（PPID），使用pstree 看进程树：

```
$ pstree
init─┬─acpid
     ├─atd
     ├─cron
     ├─dbus-daemon
     ├─dhclient
     ├─7*[getty]
     ├─irqbalance
     ├─2*[java───89*[{java}]]
     ├─java───65*[{java}]
     ├─java───76*[{java}]
     ├─java───80*[{java}]
     ├─java───90*[{java}]
     ├─2*[java───98*[{java}]]
     ├─java───37*[{java}]
     ├─java───115*[{java}]
     ├─mysqld_safe───mysqld───53*[{mysqld}]
     ├─nginx───4*[nginx]
     ├─ntpd
     ├─polkitd───2*[{polkitd}]
     ├─redis-server───2*[{redis-server}]
     ├─6*[redis-server───3*[{redis-server}]]
     ├─rpc.idmapd
     ├─rpc.statd
     ├─rpcbind
     ├─rsyslogd───3*[{rsyslogd}]
     ├─sshd───sshd───sshd───bash───pstree
     ├─supervisord
     ├─systemd-logind
     ├─systemd-udevd
     ├─upstart-file-br
     ├─upstart-socket-
     ├─upstart-udev-br
     └─zabbix_agentd───5*[zabbix_agentd]

```

### 进程类型
+ 交互进程
	+ 由一个Shell启动的进程
	+ 交互进程可以在前台运行，也可以在后台运行
+ 批处理进程
	+ 不与特定终端相关联，提交到等待队列顺序执行的进程
+ 守护进程
	+ 在Linux启动时初始化，需要时运行在后台的进程

### 进程启动方式

* 手工方式： 使用操作系统提供的用户接口
	* 前台
	* 后台 &
* 调度方式：按照预先指定的时间执行
	* at
	* batch
	* cron

### 启动启动流程图

![](https://pic3.zhimg.com/v2-b22d52cf0f11e56548dbcdf820fc2d2a_b.jpg)	

### 进程管理常用命令
 1. top/htop 查看各个状态
 2. ps 列出运行中的进程
 3. pstree 打出进程树
 4. kill 杀进程
 5. pgrep 根据名字查找进程，返回PID
 6. pkill / killall 根据名字杀进程
 7. renice 调整运行中的进程的优先级
 8. xkill 使用图形化工具杀进程
 
 

参考

[1] Chapter 4 Processes. https://www.tldp.org/LDP/tlk/kernel/processes.html

[2] 知乎: Linux进程管理 - Java3y的文章 - 知乎 https://zhuanlan.zhihu.com/p/37997035

[3] How to Manage Processes from the Linux Terminal: 10 Commands You Need to Know  https://www.howtogeek.com/107217/how-to-manage-processes-from-the-linux-terminal-10-commands-you-need-to-know/





