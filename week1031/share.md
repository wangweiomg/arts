## ARTS - Share 补2019.2.6

## JVM的守护线程和用户线程

#### 线程池创建非daemon线程

最近在看线程池时候, 发现线程池创建线程特别设置了线程为非daemon，以下是DefaultThreadFactory 中的一段代码：

```java
public Thread newThread(Runnable r) {
            Thread t = new Thread(group, r,
                                  namePrefix + threadNumber.getAndIncrement(),
                                  0);
            if (t.isDaemon())
                t.setDaemon(false);
            if (t.getPriority() != Thread.NORM_PRIORITY)
                t.setPriority(Thread.NORM_PRIORITY);
            return t;
        }
```

于是就了解了下JVM的线程。



#### 守护线程和用户线程

Java线程分类为daemon 即守护线程，和非守护线程即用户线程，守护线程一般用来辅助用户线程，如GC线程就是守护线程，

守护线程的生命周期：随着程序在JVM中运行，守护线程第一时间被启动，并且一直处于运行状态。

当所有用户线程执行完毕，程序就会杀死守护线程，离开JVM，终止程序。

也就是是说，当用户线程为0，那么JVM就会退出。

#### 守护线程的创建

其实线程 Thread类里有个 setDaemon方法，用来设置线程为守护线程：

```java
/**
     * Marks this thread as either a {@linkplain #isDaemon daemon} thread
     * or a user thread. The Java Virtual Machine exits when the only
     * threads running are all daemon threads.
     * 当所有运行的线程都是守护线程，那么JVM退出。
     
     * <p> This method must be invoked before the thread is started.
     注意这句，必须在start方法调用之前设置这个
     *
     * @param  on
     *         if {@code true}, marks this thread as a daemon thread
     *
     * @throws  IllegalThreadStateException
     *          if this thread is {@linkplain #isAlive alive}
     *
     * @throws  SecurityException
     *          if {@link #checkAccess} determines that the current
     *          thread cannot modify this thread
     */
    public final void setDaemon(boolean on) {
        checkAccess();
        if (isAlive()) {
            throw new IllegalThreadStateException();
        }
        daemon = on;
    }
```

另外守护线程创建的都是守护线程，当所有线程都为守护线程时候，JVM就会退出，下面我们验证一下：



```java
import java.util.Scanner;
import java.util.concurrent.TimeUnit;

public class DaemonThreadTest extends Thread {


    public static void main(String[] args) {

        DaemonThreadTest test = new DaemonThreadTest();
        test.setDaemon(true);
        test.start();

        Scanner sc = new Scanner(System.in);
        sc.next();

        Runtime.getRuntime().addShutdownHook(new Thread(() -> System.out.println("JVM exits!")));


    }

    @Override
    public void run() {

        for (int i = 0; i < 100; i++) {
            try {
                TimeUnit.SECONDS.sleep(1);

                Thread t = new Thread(() -> System.out.println("I am inner thread"));

                System.out.println(" inner thread isDaemon = " + t.isDaemon());
                t.start();

            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }

    }
}
```

以上逻辑很简单：

DaemonThreadTest 作为一个Daemon线程，它不停的创建线程，在主线程里加了个控制台输入的Scanner ，相当于用户线程，只要不输入，就不会退出，最后控制台打印的都是 daemon=true, 当手动输入字符回车之后，打印 JVM退出。