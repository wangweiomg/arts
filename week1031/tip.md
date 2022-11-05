## ARTS - Tip 补 2019.2.6

## 线程池的getTask()方法

#### 线程池持有线程不退出的思路

学线程池，都知道它的好处就是减少了频繁创建、销毁线程的开销，从而提高效率的，那么到底是怎么实现这个机制的呢？

我们自己设想下，线程的声明周期就是创建、就绪、运行、销毁。如果线程一直在运行状态并且可以接收其他任务，那么其实就相当于是避开了频繁创建、销毁的开销。那么，线程池也是这么个思路。

先看代码 runWorker, 我们知道runWorker其实就是相当于线程的run方法， 同理addWorker 可以看做是创建线程。

#### runWorker 

```java
final void runWorker(Worker w) {
        Thread wt = Thread.currentThread();
        Runnable task = w.firstTask;
        w.firstTask = null;
        w.unlock(); // allow interrupts
        boolean completedAbruptly = true;
        try {
        		/** 注意这个while循环， task为空时候就调用了getTask()方法，
          	* 这个getTask()方法其实就充当了获取待执行任务的功能
          	* 还有个重要点是看这个while循环里的最后，重置 task = null, 
          	* 这时候就相当于调用getTask()获取任务了,接下来我们看getTask()
          	*/
            while (task != null || (task = getTask()) != null) {
                w.lock();
                // If pool is stopping, ensure thread is interrupted;
                // if not, ensure thread is not interrupted.  This
                // requires a recheck in second case to deal with
                // shutdownNow race while clearing interrupt
                if ((runStateAtLeast(ctl.get(), STOP) ||
                     (Thread.interrupted() &&
                      runStateAtLeast(ctl.get(), STOP))) &&
                    !wt.isInterrupted())
                    wt.interrupt();
                try {
                    beforeExecute(wt, task);
                    Throwable thrown = null;
                    try {
                        task.run();
                    } catch (RuntimeException x) {
                        thrown = x; throw x;
                    } catch (Error x) {
                        thrown = x; throw x;
                    } catch (Throwable x) {
                        thrown = x; throw new Error(x);
                    } finally {
                        afterExecute(task, thrown);
                    }
                } finally {
                  	// 这里重置了task 为 null，循环继续走task = getTask()的条件
                    task = null;
                    w.completedTasks++;
                    w.unlock();
                }
            }
            completedAbruptly = false;
        } finally {
            processWorkerExit(w, completedAbruptly);
        }
    }

```



#### getTask()方法

我们看源码：

```java
private Runnable getTask() {
        boolean timedOut = false; // Did the last poll() time out?

        for (;;) {
            int c = ctl.get();
            int rs = runStateOf(c);

            // Check if queue empty only if necessary.
            if (rs >= SHUTDOWN && (rs >= STOP || workQueue.isEmpty())) {
                decrementWorkerCount();
                return null;
            }

            int wc = workerCountOf(c);

            // Are workers subject to culling?
          	// 如果设置了核心线程超时退出 或者线程数大于核心线程大小，timed 就是true
            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;

            if ((wc > maximumPoolSize || (timed && timedOut))
                && (wc > 1 || workQueue.isEmpty())) {
                if (compareAndDecrementWorkerCount(c))
                    return null;
                continue;
            }

            try {
               /* 最重要的部分看这里,这做了一个判断就是 
               * 1. timed == true, 走 workQueue.pool() 方法，
               * 2. timed == false, 走 workQueue.task() 方法
               */
                Runnable r = timed ?
                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                    workQueue.take();
                if (r != null)
                    return r;
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }
```

以上代码我们得到结果：

1. timed== true 其实就是非核心线程处理和设置了核心线程超时的核心线程处理， 走poll()方法
2. timed == false 其实就是核心线程处理， 走 take() 方法

所以实际上核心线程和非核心线程的区别(设置了超时时间的其实就是当做非核心线程处理了)  就是获取阻塞队列中方式不一样的区别了。

pool 取得队列中元素，没有就返回null， take没有就阻塞，不返回，根据这个特性，我们就发现了在runWorker里的getTask() 方法在队列为空时候就会被一直被阻塞，也就是实现了runWorker卡在那不退出的情况，也就是线程一直处在 running 的状态，等到取得新的任务，线程继续执行。



#### 简单总结

线程池实现方式：

1. Execute 方法提交任务

1. addWorker 相当于线程创建，并调用了start方法，这相当于创建了一大堆线程，
2. 每个线程执行自己的 runWorker() 方法，runWorker方法本质就是死循环从队列中取任务，当都取不到任务时候，就相当于队列为空，非核心线程获取到null 任务，核心线程获取不到任务一直被阻塞， 
3. 非核心线程继续走完runWorker() 方法，线程结束退出。





#### poll 和take

这里说下poll和take的不同，这里看下LinkedBlockingQueue的实现：

```java
public E poll(long timeout, TimeUnit unit) throws InterruptedException {
        E x = null;
        int c = -1;
        long nanos = unit.toNanos(timeout);
        final AtomicInteger count = this.count;
        final ReentrantLock takeLock = this.takeLock;
        takeLock.lockInterruptibly();
        try {
        	// 当取不到元素时候，就延迟 返回null
            while (count.get() == 0) {
                if (nanos <= 0)
                    return null;
                nanos = notEmpty.awaitNanos(nanos);
            }
            x = dequeue();
            c = count.getAndDecrement();
            if (c > 1)
                notEmpty.signal();
        } finally {
            takeLock.unlock();
        }
        if (c == capacity)
            signalNotFull();
        return x;
    }

public E take() throws InterruptedException {
        E x;
        int c = -1;
        final AtomicInteger count = this.count;
        final ReentrantLock takeLock = this.takeLock;
        takeLock.lockInterruptibly();
        try {
          // 当取不到元素时候，就执行了await()方法，阻塞
            while (count.get() == 0) {
                notEmpty.await();
            }
            x = dequeue();
            c = count.getAndDecrement();
            if (c > 1)
                notEmpty.signal();
        } finally {
            takeLock.unlock();
        }
        if (c == capacity)
            signalNotFull();
        return x;
    }
```

那么，take方法 notEmpty.await() 又是什么呢？跟上去，代码如下

```java
private final Condition notEmpty = takeLock.newCondition();

// 那么，什么是Condition呢？我们继续看newCondition()
// 这个是在ReentrantLock类里的
public Condition newCondition() {
        return sync.newCondition();
}
final ConditionObject newCondition() {
            return new ConditionObject();
        }
public final void await() throws InterruptedException {
            if (Thread.interrupted())
                throw new InterruptedException();
            Node node = addConditionWaiter();
            int savedState = fullyRelease(node);
            int interruptMode = 0;
            while (!isOnSyncQueue(node)) {
                LockSupport.park(this);
                if ((interruptMode = checkInterruptWhileWaiting(node)) != 0)
                    break;
            }
            if (acquireQueued(node, savedState) && interruptMode != THROW_IE)
                interruptMode = REINTERRUPT;
            if (node.nextWaiter != null) // clean up if cancelled
                unlinkCancelledWaiters();
            if (interruptMode != 0)
                reportInterruptAfterWait(interruptMode);
        }

```

至于这一大段 await 到底是什么意思，留个坑，以后再填吧，(我也不懂)。