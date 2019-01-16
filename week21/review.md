## ARTS - Review 补12.3号
### Linux进程那些事（2）

* briefly adv. 简单地
* made up of  由...组成
* fundamentally adv. 根本上
* Foreground 前景
* interactive adj.互动的
* spontaneously adv. 自然地
* via prep. 通过
* partially adv. 部分地
* preempt vt. 先占，取代
* relinquish 放弃


> A process refers to a program in execution; it’s a running instance of a 
> program. It is made up of the program instruction, data read from files, other 
> programs or input from a system user. 

一个进程代表一个程序执行过程；是一个程序运行实例。它由程序命令，从文件读取的数据或其他程序或系统用户输入读取数据组成。


> There are fundamentally two types of processes in Linux:

基本上Linux有两种类型的进程

* 前台进程 ，也叫交互进程，通过终端初始化控制。
* 后台进程，也叫非交互进程，不需要用户输入

什么是守护进程

一种特殊的后台运行进程随着系统启动，作为一种服务持续运行，不会死亡。可以被用户通过初始化进程进行控制。


![](https://www.tecmint.com/wp-content/uploads/2017/03/ProcessState.png)


> All processes run partially in user mode and partially in system mode

所有的进程运行时部分使用用户模式部分部分使用系统模式。

> In Linux, processes do not preempt the current, running process, they cannot stop it from running so that they can run. Each process decides to relinquish the CPU that it is running on when it has to wait for some system event. 

在Linux中，进程不会抢占当前运行中的进程，他们不会阻止它运行以便自己运行。每个进程决定放弃正在运行着的CPU当它不得不等待一些系统事件的时候。




