## ARTS - Tip  补 2019.2.13

## linux前后台切换

#### ctrl + z 和 bg 、fg

最近使用到了Linux命令的前后台切换功能。本来是命令是在前台执行，但是执行时间过长，就想要调到后台执行，于是就找了这一块的知识。

1. Linux终端，执行命令后加 & 可以让程序在后台运行。注意的是，如果退出终端，程序也停止运行，要永久运行使用 nohup， 后面再讲。

2. 如果程序已经在前台运行，可以用 ctrl+z 调到后台，但是此时程序是暂停的，需要用bg %n 放到后台继续运行

   ```shell
   ## 前台启动了一个Java程序
   java -jar push-1.0.0-GA.jar
   
   ## 使用ctrl+z 放到后台，但是此时是挂起的，要想让他在后台运行，就要用 bg %n 
   ^Z
   [1]  + 5618 suspended  java -jar push-1.0.0-GA.jar
   
   ## 后台运行
   bg %1
   ```

3. 对于所有运行的程序，可以用 jobs -l 查看

   ```shell
   ## 查看所有运行中的程序
   jobs -l
   [1]  + 5618 running    java -jar push-1.0.0-GA.jar
   ```

4. 用 fg %n 把后台程序调到前台 

   ```shell
   ## 后台调到前台
   fg %1
   fg %1
   [1]  + 5618 running    java -jar push-1.0.0-GA.jar
   
   ```

5. 终止后台命令 kill %n

   ```shell
   ## 终止
   kill %1
   2019-05-17 15:37:39.125  INFO 5618 --- [       Thread-3] o.s.s.concurrent.ThreadPoolTaskExecutor  : Shutting down ExecutorService 'applicationTaskExecutor'
   
   [1]  + 5618 exit 143   java -jar push-1.0.0-GA.jar
   ```

   

#### mysql 终端的妙用

很明显，如果需要登录终端看MySQL，那么这套操作就很有用了，如果不想开太多窗口，也不想频繁登录登出，可以登录使用完后直接 ctrl + z 调到后台，需要了再fg %n回来

```shell
## 用完就 ctrl+z
[1]  + 21841 suspended  mysql -uroot -proot

## 再用就fg %1
fg %1
```



#### nohup 的使用

我们已经知道了使用 & 后台运行，关闭终端退出账号程序也会停止，那么如果我们想要一个程序后台持续运行下去，就使用到了nohup。

nohup 是 no hang up ， 不挂断的意思

```shell
## 不挂断执行command, 输出到myout.file文件
nohup command > myout.file 2>&1 &
```

上面的例子， 0 是stdin， 标准输入， 1 是stdout 标准输出， 2是stderr 标准错误

2 > &1 是将标准错误重定向到 标准输出 &1 里，标准输出&1 再被重定向到myout.file里。

#### nohup 和 &  的区别

& 是在后台运行。

Nohup 是不挂断运行，注意并不是后台运行，最后加了个 & 一起，表示不间断的后台运行。

1. sh test.sh & .  将 test.sh任务放到后台，关闭终端，任务停止
2.  nohup sh test.sh  将test.sh放到前台运行，输出到nohup.out 文件，注意这时候使用 ctrl+z ，bg %n是可以放到后台运行的，而且退出终端不会停止任务
3. Nohup sh test.sh &   将test.sh 放到后台运行，输出到nohup.out文件，退出不会停止任务，和2放后台一样。

2 和3 都不会输出日志到终端屏幕，直接输出到了nohup.out



#### **结论**

使用**&**后台运行程序：

- 结果会输出到终端
- 使用Ctrl + C发送SIGINT信号，程序免疫
- 关闭session发送SIGHUP信号，程序关闭

使用**nohup**运行程序：

- 结果默认会输出到nohup.out
- 使用Ctrl + C发送SIGINT信号，程序关闭
- 关闭session发送SIGHUP信号，程序免疫

**平日线上经常使用nohup和&配合来启动程序**：

- 同时免疫SIGINT和SIGHUP信号

同时，还有一个最佳实践：

- 不要将信息输出到终端标准输出，标准错误输出，而要用日志组件将信息记录到日志里