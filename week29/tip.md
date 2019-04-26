## ARTS - Tip 补2019.1.23
## 线程池的execute 方法

### 源码

线程池提交两种任务，一种是无返回值的，就是execute, 另一种是有返回值的submit,下次讲。

先来看源码：

```java

public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * 三步处理:
         * 1. 如果比核心线程数小，会尝试打开一个新线程来执行任务。调用addWorker自动检查runState状态和workerCount 运行中的任务数，添加失败返回false.
         * 2. 如果一个任务可以入队成功，仍要二次校验是否添加成功(因为存在上次检查过后就消亡了)。
         * 3. 如果任务不能入队，尝试添加一个新线程，如果失败，我们知道线程池关闭或队列饱和，拒绝任务。
         */

				// 获得线程池(包含状态 + 数量)
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

### 辅助方法：workerCountOf(c) , isRunning(c)

在看addWorker之前，先看看到底怎么获得工作线程数和状态的，我们已知到32位原子变量的前三位是状态，后29位是数量：

```java
		// Packing and unpacking ctl
		// 对后29位数量位取反后按位与， 获得状态
    private static int runStateOf(int c)     { return c & ~CAPACITY; }
    // 直接 32位 和29 位按位与，获得后29位数字(32位前三位000)
    private static int workerCountOf(int c)  { return c & CAPACITY; }
		// 按位或，获得包装后的线程池代数
    private static int ctlOf(int rs, int wc) { return rs | wc; }

		// 比SHUTDOWN 小的只能是RUNNING了
		private static boolean isRunning(int c) {
        return c < SHUTDOWN;
    }

```



### addWorker

再来看最重要的addWorker方法

```java
/**
      检查一个任务是否被正常添加成功不会越界。如果成功worker 数量增加，如果可能一个新
      线程创建启动，该任务作为线程第一个任务执行。
      如果线程池关闭或正在shutdown, 返回false.
      如果线程工厂创建线程失败，也返回false.
      如果线程创建失败，无论是线程工厂返回null或异常(OOM错误在Thread.start())，直接回滚。
     *
     
       @param firstTask 新线程应该第一个运行的任务。当线程数小于核心线程 任务会入队，队列满时候，
       非核心线程创建执行任务。
       @param core 如果true, 使用corePoolSize 作为边界，否则使用maximumPoolSize.
     * @return 添加成功返回true
     */
    private boolean addWorker(Runnable firstTask, boolean core) {
      	// 死循环标签
        retry:
        for (;;) { // 死循环
          
          	// 获得当前池子情况
            int c = ctl.get();
            int rs = runStateOf(c);

            // Check if queue empty only if necessary.
          	// 1. 如果不是RUNNING状态， rs > SHUTDOWN 直接返回 false,不接受新线程
          	// 2. 如果是 SHUTDOWN 状态， 并且firstTask != null 那么还是返回false
          	// 3. 如果是 SHUTDOWN 状态， 并且firstTask == null , 而且 workQueue不为空，
          	// 	可以增加 first==null 的worker，来消耗队列中的任务。
          	// 结论：
          	// rs > SHUTDOWN ，说明是STOP或以上状态，会关闭所有线程同时移除QUEUE中所有待执行线程的，所以不需要增加 firstWorker == null 的Worker了。
          	// SHUTDOWN 状态下，只能增加 firstWorker == null 的线程，主要为了消耗 队列中的任务
          	
            if (rs >= SHUTDOWN &&
                ! (rs == SHUTDOWN &&
                   firstTask == null &&
                   ! workQueue.isEmpty()))
                return false;

          	// RUNNING状态继续往下走，进入内部死循环
            for (;;) {
              	// 获得线程数
                int wc = workerCountOf(c);
              	// 如果线程数大于最大容量 或者 大于核心线程数 或者 设置的最大线程数，返回false
                if (wc >= CAPACITY ||
                    wc >= (core ? corePoolSize : maximumPoolSize))
                    return false;
              	// 原子操作，增加线程数，增加成功，跳出 retry 外部死循环
                if (compareAndIncrementWorkerCount(c))
                    break retry;
              	// 如果线程增加失败, 获得线程状态，如果还是rs状态，就继续增加尝试
              	// 如果已经不是rs状态，跳出内部循环，去外部检查状态再说
                c = ctl.get();  // Re-read ctl
                if (runStateOf(c) != rs)
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }

      	// 走到这里说明已经成功记录了线程数
      
      	// 初始化 线程启动状态为false, 线程添加状态为false
        boolean workerStarted = false;
        boolean workerAdded = false;
      	// 这个 Worker 其实就是 Thread
        Worker w = null;
        try {
          	// 这里创建线程, 创建过程我们先按下不表。
          	/**
          	* Worker(Runnable firstTask) {
              setState(-1); // inhibit interrupts until runWorker
              this.firstTask = firstTask;
              this.thread = getThreadFactory().newThread(this);
        			}
          	*/
            w = new Worker(firstTask);
            final Thread t = w.thread;
          	// 线程创建成功的情况下
            if (t != null) {
              	// 加锁操作
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                  	// 再次检查 获得锁期间是否发生了状态改变
                    int rs = runStateOf(ctl.get());

                  	// 只能是 RUNNING状态，或者SHUTDOWN状态同时 firstTask为空 情况才能走下边
                    if (rs < SHUTDOWN ||
                        (rs == SHUTDOWN && firstTask == null)) {
                      	// 检查线程任务 t 是否在等待启动，不是就抛异常
                        if (t.isAlive()) // precheck that t is startable
                            throw new IllegalThreadStateException();
                      	// 添加任务到HashSet结构
                        workers.add(w);
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                        workerAdded = true;
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }

/**
     * Rolls back the worker thread creation.
     * - removes worker from workers, if present
     * - decrements worker count
     * - rechecks for termination, in case the existence of this
     *   worker was holding up termination
     	添加worker失败，就回滚操作
     	1. 删除wokers里添加的wokers
     	2. 减少 worker 数量
     	3. 再次检查，防止在termination还持有workers
     */
    private void addWorkerFailed(Worker w) {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            if (w != null)
                workers.remove(w);
            decrementWorkerCount();
            tryTerminate();
        } finally {
            mainLock.unlock();
        }
    }
```

