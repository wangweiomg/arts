## ARTS - Share 
## 关于 ThreadPoolExecutor 方式创建线程

我们知道在使用多线程时候，使用线程池技术能够减少系统在创建、销毁线程时的开销，提高性能.
一般用Executors的四个方法：

+ newCachedThreadPool
+ newFixedThreadPool
+ newScheduledThreadPool
+ newSingleThreadExecutor


### Executors 方式
在创建固定数量的线程池时候是：
#### 1. newFixedThreadPool()

```
ExecutorService exec = Executors.newFixedThreadPool()
```

我们查看源码就是：

```
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
```

返回创建了ThreadPoolExecutor

```
public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
    }
    
    
     /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.acc = System.getSecurityManager() == null ?
                null :
                AccessController.getContext();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }

```

**各个参数的意义：（面试经常问）**

+ corePoolSize 核心线程数大小，线程池中保持存活的线程个数，当线程数 < corePoolSize 时候会创建线程执行runnable
+ maximumPoolSize 线程池中允许的最大线程数，当线程数 >= corePoolSize 时候，会把runnable放入workQueue中 
+ keepAliveTime 保持存活时间，当线程数 > 核心线程数时，空闲线程在被执行前保持最大的存活时间
+ unit  时间单位
+ workQueue 保存未执行任务的阻塞工作队列
+ threadFactory 线程创建工厂
+ handler 线程被阻塞的拒绝策略


然后我们继续看其他的线程池
#### 2. newSingleThreadExecutor

```
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
    
public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory) {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>(),
                                    threadFactory));
    }

```

也是使用了ThreadPoolExecutor 来构造, corePoolSize和maximusPoolSize 都是1， workQueue是LinkedBlockingQueue。 说明他是相当于newFixedThreadPool(1)


#### 3. newCachedTreadPool

```
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
    
 public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
    }
```
这个也是用ThreadPoolExecutor 构造的， 核心线程是0， 最大线程数是Integer.MAX_VALUE， 保持存活60秒， 用的SynchronousQueue 队列。


### 任务执行顺序

1. 当线程数小于corePoolSize时，创建线程执行任务
2. 当线程数大于等于corePoolSize并且workQueue没有满，放入workQueuq
3. 线程数大于等于 corePoolSize并且 workQueue满了，新任务新建线程运行，线程总数要小于maximumPoolSize
4. 当线程数总数等于maximumPoolSize	并且workQueue满了的时候执行handler的rejectedException.拒绝策略


JDK提供的阻塞队列：

1. ArrayBlockingQueue: 数组结构的有界阻塞队列
2. LinkedBlockingQueue: 链表结构的有界阻塞队列
3. PriorityBlockingQueue 支持优先级排序的无界阻塞队列
4. DelayQueue 使用优先级队列实现的无界阻塞队列
5. SynchronousQueue 一个不存储元素的阻塞队列
6. LinkedTransferQueue: 链表结构无界阻塞队列
7. LinkedBlockingDeque 链表结构双向阻塞队列

#### 阻塞队列
阻塞队列是一个在队列基础上支持两个附加操作的队列

2个附加操作

+ 插入： 队列满时，队列会阻塞插入元素的线程，直到队列不满
+ 移除： 队列空时，获取元素的线程会等待队列变为非空


#### 阻塞队列应用场景
阻塞队列常用于生产者和消费者的场景，生产者是向队列里添加元素的线程，消费者是从队列里取元素的线程。简而言之，阻塞队列是生产者用来存放元素、消费者获取元素的容器。








