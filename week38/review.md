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





