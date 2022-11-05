## ARTS - Tip 补 2019.1.16
## 线程池的状态

我们来看 ThreadPoolExecutor 的execute方法。

线程池提交方法有两个， execute 和 submit, execute 提交的是无返回值的Runnable, submit提交的是有返回值 Callable.今天先看execute.

```
/**

     * 
     * 在将来某个时刻执行给定的任务，可能在一个新的线程里或池中存在的的线程里。
     * 如果任务不能被提交执行，这个执行器已经被当前 rejectExecutionHandler 停止
     *
     *
     * @param command 待执行的任务
     * @throws RejectedExecutionException 如果任务不被接受执行，抛出这个异常
     */
     
    public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * Proceed in 3 steps:
         *
         * 1. 如果运行线程少于corePoolSize, 会尝试启动一个新线程首先执行这个任务。
         * 调用 addWork 原子检查 运行状态 和 工作线程数，避免返回false不能添加线程的情况
         * 
         * 2. 如果一个任务能被成功进入队列， 之后我们仍需二次验证我们是否应该添加一个线程
         * (因为存在存在上次检查线程死亡的情况)或者在实体进入到这个方法时 池子关闭了。所以
         * 我们重新状态检查 如果必要回滚入队实体如果停止了，或者没有的话启动一个新线程
         * 
         * 3. 如果我们不能够入队任务，我们尝试添加一个新的线程。如果失败了，我们知道关闭了
         * 或者饱和了所以拒绝了任务。
         */
        int c = ctl.get();
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }
```

首先要先说明ctl这个状态变量：

```
/**
     * ctr 是一个原子量，包含两个属性 workerCount 和 runState.
     * workerCount 表示当前有效的线程数，也就是Worker的数量
     * runState 表示的线程池状态， 运行、关闭等。。

     * 
     * 为了把它俩打包成一个int 值，我们限制 workerCount为 (2^29-1) (大约5亿) 个线程
     * 而不是 (2^31)-1 (大约20亿) 可表示的。
     * workerConunt 是已经被提交启动不允许停止的worker 数量。数值可能和实际活跃线程数
     * 稍有不同，例如，当一个线程工厂请求时创建线程失败，退出时在线程停止前记录了数量。用户
     * 可见线程池大小是当前工作数大小。
     * 
       *runState 是整个线程池生命周期，有如下值：
       1. RUNNING 可以添加任务，处理在队列中的任务
       2. SHUTDOWN 不接受新任务，但是处理完在队列中的任务
       3. STOP 不接受新任务，也不处理队列中的任务，中断正在处理的任务。
       4. TIDYING 所有任务结束，workerCount 为0, 
       5. terminated()方法结束
     *
     *   RUNNING:  Accept new tasks and process queued tasks
     *   SHUTDOWN: Don't accept new tasks, but process queued tasks
     *   STOP:     Don't accept new tasks, don't process queued tasks,
     *             and interrupt in-progress tasks
     *   TIDYING:  All tasks have terminated, workerCount is zero,
     *             the thread transitioning to state TIDYING
     *             will run the terminated() hook method
     *   TERMINATED: terminated() has completed
     *
     * The numerical order among these values matters, to allow
     * ordered comparisons. The runState monotonically increases over
     * time, but need not hit each state. The transitions are:
     * 状态转化主要是：
     * RUNNING -> SHUTDOWN(调用shutdown())
     *    On invocation of shutdown(), perhaps implicitly in finalize()
     * (RUNNING or SHUTDOWN) -> STOP (调用 shutdownNow())
     *    On invocation of shutdownNow()
     * SHUTDOWN -> TIDYING queue队列和 pool都为空
     *    When both queue and pool are empty
     * STOP -> TIDYING pool 为空
     *    When pool is empty
     * TIDYING -> TERMINATED 调用 terminated() 
     *    When the terminated() hook method has completed
     *
     * Threads waiting in awaitTermination() will return when the
     * state reaches TERMINATED.
     *
     * Detecting the transition from SHUTDOWN to TIDYING is less
     * straightforward than you'd like because the queue may become
     * empty after non-empty and vice versa during SHUTDOWN state, but
     * we can only terminate if, after seeing that it is empty, we see
     * that workerCount is 0 (which sometimes entails a recheck -- see
     * below).
     */
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
```

再来看状态码：

```
// 低29位是线程池容量，高3位是线程状态
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
	// 设定偏移量 29
    private static final int COUNT_BITS = Integer.SIZE - 3;
    // 设置最大容量 2^29 -1
    private static final int CAPACITY   = (1 << COUNT_BITS) - 1;

	// 几个状态， 用 Integer 的高三位表示
    // runState is stored in the high-order bits
    // 111
    private static final int RUNNING    = -1 << COUNT_BITS;
    // 000
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    // 001
    private static final int STOP       =  1 << COUNT_BITS;
    // 010
    private static final int TIDYING    =  2 << COUNT_BITS;
    // 011
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    // 获取线程池状态，取前三位
    private static int runStateOf(int c)     { return c & ~CAPACITY; }
    // 获取当前正在工作的worker ，主要是取后29位
    private static int workerCountOf(int c)  { return c & CAPACITY; }
    // 获取 ctl
    private static int ctlOf(int rs, int wc) { return rs | wc; }
```

