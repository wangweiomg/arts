## Review

### Stop the World Event

> All the Garbage Collections are “Stop the World” events because all application threads are stopped until the operation completes. Since Young generation keeps short-lived objects, Minor GC is very fast and the application doesn’t get affected by this. However, Major GC takes a long time because it checks all the live objects. Major GC should be minimized because it will make your application unresponsive for the garbage collection duration. So if you have a responsive application and there are a lot of Major Garbage Collection happening, you will notice timeout errors. The duration taken by garbage collector depends on the strategy used for garbage collection. That’s why it’s necessary to monitor and tune the garbage collector to avoid timeouts in the highly responsive applications.

所有的垃圾回收会停止世界事件，因为所有的应用线程在操作完成前都会停止。...

垃圾回收器消耗时长取决于垃圾回收策略。这就是有必要监控垃圾回收的原因，避免在高响应应用超时。



## Java 内存模型 - 永久代

### Java Memory Model - Permanent Generation

> Permanent Generation or “Perm Gen” contains the application metadata required by the JVM to describe the classes and methods used in the application. Note that Perm Gen is not part of Java Heap memory. Perm Gen is populated by JVM at runtime based on the classes used by the application. Perm Gen also contains Java SE library classes and methods. Perm Gen objects are garbage collected in a full garbage collection.

永久代 后者“Perm Gen” 包括应用元数据 metadata 被JVM要求用来描述应用中类和方法使用情况。注意perm Gen 不是Java对内存的一部分。Perm Gen 在JVM运行期基于被 应用使用的类频繁使用到。Perm Gen也包括Java SE包的类和方法。Perm Gen 对象在full gc时候被回收。

### Java 内存模型  - 方法区

> Method Area is part of space in the Perm Gen and used to store class structure (runtime constants and static variables) and code for methods and constructors.

方法区是Perm Gen一部分用来存储类结构(运行时常量，静态变量) 和方法构造器代码。

### Java 内存模型 - 内存池

> Memory Pools are created by JVM memory managers to create a pool of immutable objects if the implementation supports it. String Pool is a good example of this kind of memory pool. Memory Pool can belong to Heap or Perm Gen, depending on the JVM memory manager implementation.

内存池被JVM内存管理创建来 创建一个不可变对象的池子，如果实现支持的话。String 池是这类内存池的好的例子。内存池可以属于堆，也可以属于Perm Gen. 取决于JVM 内存管理实现。

### Java内存模型 - 运行时常量池

> Runtime constant pool is per-class runtime representation of constant pool in a class. It contains class runtime constants and static methods. Runtime constant pool is part of the method area.

运行时常量池是一个类中常量池的运行时表示。包括类运行时常量 和静态方法。运行时常量池是方法区的一部分。

### Java内存模型 - Java 栈内存

> Java Stack memory is used for execution of a thread. They contain method specific values that are short-lived and references to other objects in the heap that is getting referred from the method. You should read [Difference between Stack and Heap Memory](https://www.digitalocean.com/community/tutorials/java-heap-space-vs-stack-memory).

Java栈内存用来执行线程。包括方法明确值，短暂生存和引用给其他堆中对象的，方法的引用。

### Java内存管理 - Java堆内存设置

> Java provides a lot of memory switches that we can use to set the memory sizes and their ratios. Some of the commonly used memory switches are:

Java踢动很多内存设置，我们可以使用设置内存大小和比例。

| VM Switch         | desc                                                         |
| :---------------- | ------------------------------------------------------------ |
| -Xms              | 设置JVM 启动时候初始化对内存大小                             |
| -Xmx              | 设置JVM最大堆内存大小                                        |
| -Xmn              | 设置年轻代大小，剩下的区域就是老年代                         |
| -XX:PermGen       | 设置永久代初始化大小                                         |
| -XX:MaxPermGen    | 设置永久代最大尺寸                                           |
| -XX:SurvivorRatio | 设置Eden:Survivor比例，例如年轻代是10m, 这里设置-XX:SurviorRatio=2 ， 那么5m分配给Eden, 2.5m给两个Survivor. 默认值是8 。 就是 Eden 8   s0:1 s1: 1 |
| --XX:NewRatio     | 设置 old/new 比例。默认是2                                   |

大多数时候，上面选项就够了，如果你需要其他选项就看[官方](https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html)吧 

### Java内存管理 - Java垃圾回收

> Java Garbage Collection is the process to identify and remove the unused objects from the memory and free space to be allocated to objects created in future processing. One of the best features of Java programming language is the **automatic garbage collection**, unlike other programming languages such as C where memory allocation and deallocation is a manual process. **Garbage Collector** is the program running in the background that looks into all the objects in the memory and find out objects that are not referenced by any part of the program. All these unreferenced objects are deleted and space is reclaimed for allocation to other objects. One of the basic ways of garbage collection involves three steps:

Java垃圾回收是一个过程，用来定位删除内存的无用对象， 和给未来分配给对象内存空间的进程腾出空间。Java编程语言的一个最好的特点是自动垃圾回收，不像C等其他需要手动分配回收内存的语言。垃圾收集器是后台运行的程序，检查内存所有对象找出没有被程序引用的对象。所有这些未被引用的对象会被删除，空间重新分配给其他对象。垃圾回收基本方式有三步：

> 1. **Marking**: This is the first step where garbage collector identifies which objects are in use and which ones are not in use.
> 2. **Normal Deletion**: Garbage Collector removes the unused objects and reclaim the free space to be allocated to other objects.
> 3. **Deletion with Compacting**: For better performance, after deleting unused objects, all the survived objects can be moved to be together. This will increase the performance of allocation of memory to newer objects.
>
> 1. 标记。 这是垃圾护回收第一步，确定对象哪些在使用，哪些不在使用
> 2. 正常删除：垃圾回收期删除无用对象，回收内存空间准备分配给其他对象。
> 3. 压缩删除：为了更好的性能， 删除无用对象后，所有的存活对象移动到一起。这家宁辉提高分配新对象的性能。

> There are two problems with a simple mark and delete approach.
>
> 1. First one is that it’s not efficient because most of the newly created objects will become unused
> 2. Secondly objects that are in-use for multiple garbage collection cycle are most likely to be in-use for future cycles too.

这里的简单标记删除方法有两个问题：

1. 第一个是不够高效，以为大量的新创建对象很快变无用
2. 第二，那些多次垃圾回收还在用的对象，很大可能在未来垃圾回收时还在使用。

> The above shortcomings with the simple approach is the reason that **Java Garbage Collection is Generational** and we have **Young Generation** and **Old Generation** spaces in the heap memory. I have already explained above how objects are scanned and moved from one generational space to another based on the Minor GC and Major GC.

以上简单清理的缺点就是Java垃圾回收分代的原因，就是堆内存中有年轻代和老年代。我已经解释过对象如何扫描和移动从一个区域空间到另一个区域基于minor GC 和 Major GC



### Java内存管理 - Java垃圾回收类型

> There are five types of garbage collection types that we can use in our applications. We just need to use the JVM switch to enable the garbage collection strategy for the application. Let’s look at each of them one by one.

这有五种我们可以在应用中使用的垃圾回收类型。我们只需要使用JVM switch 来为应用启用垃圾回收策略。来一个个看

> 1. **Serial GC (-XX:+UseSerialGC)**: Serial GC uses the simple **mark-sweep-compact** approach for young and old generations garbage collection i.e Minor and Major GC.Serial GC is useful in client machines such as our simple stand-alone applications and machines with smaller CPU. It is good for small applications with low memory footprint.
> 2. **Parallel GC (-XX:+UseParallelGC)**: Parallel GC is same as Serial GC except that is spawns N threads for young generation garbage collection where N is the number of CPU cores in the system. We can control the number of threads using `-XX:ParallelGCThreads=n` JVM option.Parallel Garbage Collector is also called throughput collector because it uses multiple CPUs to speed up the GC performance. Parallel GC uses a single thread for Old Generation garbage collection.
> 3. **Parallel Old GC (-XX:+UseParallelOldGC)**: This is same as Parallel GC except that it uses multiple threads for both Young Generation and Old Generation garbage collection.
> 4. **Concurrent Mark Sweep (CMS) Collector (-XX:+UseConcMarkSweepGC)**: CMS Collector is also referred as concurrent low pause collector. It does the garbage collection for the Old generation. CMS collector tries to minimize the pauses due to garbage collection by doing most of the garbage collection work concurrently with the application threads.CMS collector on the young generation uses the same algorithm as that of the parallel collector. This garbage collector is suitable for responsive applications where we can’t afford longer pause times. We can limit the number of threads in CMS collector using `-XX:ParallelCMSThreads=n` JVM option.
> 5. **G1 Garbage Collector (-XX:+UseG1GC)**: The Garbage First or G1 garbage collector is available from Java 7 and its long term goal is to replace the CMS collector. The G1 collector is a parallel, concurrent, and incrementally compacting low-pause garbage collector.Garbage First Collector doesn’t work like other collectors and there is no concept of Young and Old generation space. It divides the heap space into multiple equal-sized heap regions. When a garbage collection is invoked, it first collects the region with lesser live data, hence “Garbage First”. You can find more details about it at [Garbage-First Collector Oracle Documentation](https://docs.oracle.com/javase/7/docs/technotes/guides/vm/G1.html).

1. Serial GC (-XX:+UseSerialGC): Serial GC 使用简单的标记清除方式来为新生代和老年代来及回收。Minor 和Major GC Serial GC 在客户端单机应用和小CPU的情况很有用。对低内存占用的小应用很友好
2. Parallel GC (-XX:+UseParallelGC): Parallel GC 和 SerialGC相同， 除了它会裂变N个线程来为年轻代垃圾回收，N是系统CPU核心数。我们可以控制线程数通过 ```-XX:ParallelGCThreads=n``` 。Parrallel Garbage 收集器也被叫做吞吐量收集器因为它使用多个CPU来加速GC性能。Parallel GC 使用一个单线程做老年代垃圾回收。
3. Parrallel Old GC(-XX:+UseParallelOldGC): 和Parallel GC 相同，除了它会都是用多线程来处理新生代和老年代
4. Concurrent Mark Sweep(CMS) Collector (-XX:+UseConcMarkSweepGC): CMS 收集器也被作为并发低延迟收集器。它处理老年代垃圾。CMS收集器尝试垃圾回收时最小的延迟通过和应用线程并发工作来处理大部分的垃圾回收。CMS收集器在新生代使用和parallel 收集器相同的算法(标记清除).这种垃圾收集器适合响应式应用，不能承受长时间延迟。我们可以限制CMS的线程数通过```-XX:ParallelCMSThreads=n```
5. G1 Garbage Collector(-XX:UseG1GC): 垃圾优先或G1 垃圾收集器从java7开始可用，长期目标是替换CMS收集器。G1收集器是一个并行的并发的增量压缩的地延迟垃圾收集器。G1收集器不像其他垃圾收集器，没有年轻代老年代的概念。分配堆空间为多个相同大小的区域。当垃圾回收工作，首先收集低存活区域数据，因此叫“垃圾优先”。更多细节[官方文档](https://docs.oracle.com/javase/7/docs/technotes/guides/vm/G1.html)

### Java内存管理 - Java垃圾回收监控

> We can use the Java command line as well as UI tools for monitoring garbage collection activities of an application. For my example, I am using one of the demo application provided by Java SE downloads. If you want to use the same application, go to [Java SE Downloads](https://www.oracle.com/java/technologies/javase-downloads.html) page and download **JDK 7 and JavaFX Demos and Samples**. The sample application I am using is **Java2Demo.jar** and it’s present in `jdk1.7.0_55/demo/jfc/Java2D` directory. However this is an optional step and you can run the GC monitoring commands for any java application. Command used by me to start the demo application is:

我们使用Java命令行和UI工具来监控一个应用的垃圾回收活动。例如我的例子，我使用一个Java SE 下载的demo应用。如果你想用相同的应用，去 [Java SE](https://www.oracle.com/java/technologies/downloads/)下载 JDK7 和 JavaFX例子。





