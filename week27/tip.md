## ARTS - Tip 补2019.1.9 
## 关于创建线程池

### 阿里巴巴规范
在阿里巴巴Java开发手册里有这么一条：

> 4. 【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样
的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。 

>说明:Executors 返回的线程池对象的弊端如下:

>1)FixedThreadPool 和 SingleThreadPool:
允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。 
>
>2)CachedThreadPool 和 ScheduledThreadPool:
允许的创建线程数量为 Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM。


我们使用线程池一般就是这样(如下)，Java编程思想里面的也这么举例：

```
ExecutorService exec = Executors.newFixedThreadPool(10);
...

```

这里说的弊端第一条是队列堆积大量请求导致OOM，我们就看看这两个源码：

### FixedThreadPool ,SingleThreadPool

首先是 FixedThreadPool:

```
// 这里返回了ThreadPoolExecutor 对象
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
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

这里看到工作队列是 new LinkedBlockingQueue<Runnable>, 继续看这个队列源码：

```
  /**
     * Creates a {@code LinkedBlockingQueue} with a capacity of
     * {@link Integer#MAX_VALUE}.
     */
    public LinkedBlockingQueue() {
        this(Integer.MAX_VALUE);
    }
    
    
    
    /**
     * Creates a {@code LinkedBlockingQueue} with the given (fixed) capacity.
     *
     * @param capacity the capacity of this queue
     * @throws IllegalArgumentException if {@code capacity} is not greater
     *         than zero
     */
    public LinkedBlockingQueue(int capacity) {
        if (capacity <= 0) throw new IllegalArgumentException();
        this.capacity = capacity;
        last = head = new Node<E>(null);
    }

```
可以知道这个LinkedBlockingQueue的最大容量就是Integer.MAX_VALUE. 上面说法正确，确实有可能堆积的请求超过这个最大限度而导致OOM。

我们再看SingleThreadPool:

```
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
    
    
```

这里工作队列也是使用了LinkedBlockingQueue ，显然最大也是Integer.MAX_VALUE。 他的核心也是创建了有一个 ThreadPoolExecutor，后面我们详细说这个。

### CachedThreadPool 和 ScheduledThreadPool 

这两个也是说当创建线程数量大于Integer.MAX_VALUE时发生OOM异常，我们求证下:

```
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }

```
这里直接设置了最大线程数为Integer.MAX_VALUE.

```
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
        return new ScheduledThreadPoolExecutor(corePoolSize);
    }
    
    public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
    }
```





这里和CachedThreadPool一样，也是设置了最大线程数，super就是ThreadPoolExecutor

由此可知上面所言非虚。最后我们详细看看线程池创建都用到的ThreadPoolExecutor.

### ThreadPoolExecutor

首先搞明白各个参数。

```
/**
 * Creates a new {@code ThreadPoolExecutor} with the given initial
 * parameters.
 *
 * @param corePoolSize the number of threads to keep in the pool, even
 *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
核心池大小 。就是池中保持的线程数量，尽管他们是空闲的，除非 allowCoreThreadTimeOut 设置了核心线程最大空闲时间。
 *        
 * @param maximumPoolSize the maximum number of threads to allow in the
 *        pool
 *        池最大尺寸。池子允许的最大线程数。
 * @param keepAliveTime when the number of threads is greater than
 *        the core, this is the maximum time that excess idle threads
 *        will wait for new tasks before terminating.
 *        保持存活时间。当线程数超过核心数时，这些线程最大空闲时间。
 * @param unit the time unit for the {@code keepAliveTime} argument
 * 			单位。这个是keepAliveTime设置数量的单位。
 * 		 
 * @param workQueue the queue to use for holding tasks before they are
 *        executed.  This queue will hold only the {@code Runnable}
 *        tasks submitted by the {@code execute} method.
 *        工作队列。在任务被执行前持有这些任务的队列。在方法 execute 提交任务之前，队列一直持有。
 * @param threadFactory the factory to use when the executor
 *        creates a new thread
 *        线程工厂 执行器用来创建线程。
 *        
 * @param handler the handler to use when execution is blocked
 *        because the thread bounds and queue capacities are reached
			因为到达线程边界和队列容量执行被阻塞，这时候执行handler
 
 
 * @throws IllegalArgumentException if one of the following holds:<br>
 *         {@code corePoolSize < 0}<br>
 *         {@code keepAliveTime < 0}<br>
 *         {@code maximumPoolSize <= 0}<br>
 *         {@code maximumPoolSize < corePoolSize}
			当任一以下发生就抛出IllegalArgumentException 非法参数异常。
			
			corePoolSize < 0
			keepAliveTime < 0
			maximumPoolSize <= 0
			maximumPoolSize < corePoolSize

 * @throws NullPointerException if {@code workQueue}
 *         or {@code threadFactory} or {@code handler} is null

 	当 workQueue 或 threadFactory 或hander 为空时 抛出空指针异常。
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


### 分析线程池
了解了ThreadPoolExecutor ， 就知道了FixedThreadPool的  corePoolSize 和maximumPoolSize 是相等的，也就是说核心线程数等于最大线程数，超过核心线程数的空闲线程。所以， keepAliveTime就没有存在的必要了，直接设置为0.

SingleThreadExecutor和 FixedThreadPool 一样， 它是FixedThreadPool的一种形式，即coreThreadPool和maximumThreadPoll 都为1 的FixedThreadPool.


再看CachedThreadPool， 它的核心线程数为0， 最大线程数为Integer.MAX_VALUE， 非核心线程最大空闲时间为60秒。也就是说他没有保持核心线程，只要任务提交就进队列，直到达到容量，任务完成就空闲，空闲超过60秒就释放。


ScheduledThreadPool，有核心线程数，最大线程数为Integer.MAX_VALUE，没有空闲时间。
