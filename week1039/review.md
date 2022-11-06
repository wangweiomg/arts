## Review

## [理解JVM架构](https://medium.com/platform-engineer/understanding-jvm-architecture-22c0ddf09722)

> Understanding JVM architecture and how Java really works under the hood is an important learning for every Java developer in order to effectively make use of the Java ecosystem. This blog post series will provide you with a solid foundation on JVM internals and technologies around the Java ecosystem.

理解JVM架构和Java在覆盖下如何真正工作，对每个想要高效使用Java生态的Java开发者，是一件很重要的学习。本篇博客系列将会为你在JVM内部和围绕Java生态技术，提供一个坚实的基础。

### 背景

> Designed in 1995 by James Gosling for Sun Microsystems, Java is a multi-paradigm (i.e. object-oriented class-based, structural, imperative, generic, reflective, concurrent) programming language which is loved by millions of developers. On any given ranking index, Java becomes the most popular language for the past 15 years. Tens of thousands of enterprise applications developed in the last 15 years have been mostly written in Java, making it the language of choice for building enterprise-grade production software systems.

James Gosling在1995年为 Sun Microsystem设计的 Java, 是一个多范式(面向对象，基于类，结构化，重要的，通用的，可反射的，并发)编程语言 ，被百万开发者热爱。在任何排名列表，Java 过去15年成为最流行的语言。万计企业应用在过去15年大部分使用Java开发应用，让它作为构建企业级生产软件系统的语言选择。

>  Even though I have been using Java since 2015, I recently realized the power of Java ecosystem while doing my final year undergraduate research on Java performance aspects and it motivated to dig deeper into the world of Java. I am planning to write a series of blog posts related to Java internals, performance profiling, server tuning, and many more interesting topics and kindly invite you to stay in touch with this blog. And that’s it for now. Let’s start from primers on Java fundamentals!

尽管我从2015开始使用Java，我最近在做我的本科毕业研究关于Java性能和深入挖掘Java世界时，意识到Java生态的力量。我正在计划写一个系列博客关于Java内部的，性能相关，服务调整，很多有趣的主题，邀请你长期关注博客。现在，开始Java基础。

### Java环境

> For almost any programming language, you need a specific environment which comprises of all the necessary components, application programming interfaces, and libraries in order to develop, compile, debug and execute its programs. Java has 2 such environments and everyone working with Java has to start their work after setting up one of these environments on their local development or production environment platforms.
>
> - **JRE (Java Runtime Environment)**: the minimum environment needed for running a Java application (no support for developing). It includes JVM (Java Virtual Machine) and deployment tools.
> - **JDK (Java Development Kit)**: the complete development environment used for developing and executing Java applications. It includes both JRE and development tools.
>
> JRE is meant for users, while JDK is meant for programmers.

对于几乎任意编程语言，你需要一个特定的环境，由所有必须的组件、应用程序接口和开发、编译、调试、执行库组成。Java有2个这样的环境，每个用Java工作的的人必须在他们本地开发或生产环境平台建立其中一个环境。

* JRE 运行Java应用必要的最小的环境(不支持开发)。包括JVM 和部署工具。
* JDK 完整的开发环境用来开发执行Java应用。包括JRE和开发工具。

JRE面对用户， JDK面向开发者。



### Java如何工作

> You can start writing a simple Java program with any terminal editor (vim, nano) or GUI editor (gedit, sublime). For a complex Java application, you may need an IDE (Integrated Development Environment) like IntelliJ IDEA, Eclipse, or Netbeans. A typical Java program should contain correct **language syntax** and **.java** format. It is recommended to use programming concepts like OOP (Object Oriented Programming) and appropriate architectural patterns for the convenience of structuring and maintaining your Java programs.

你编写一个简单的Java程序可以使用任何终端编辑器，如vim,nano或者GUI编辑器(gedit,sublime)。 对于一个复杂的JAVA应用，你可能需要一个IDE（集成开发环境）像IntelliJ IDEA , Eclipse,或者Netbeans.一个专业的Java程序应该包含正确的语法和.java格式。推荐使用编程概念像OOP(面向对象编程)和恰当的架构模式方便的结构，来维护你的Java程序。

> The major strength of Java is, it has been designed to run on variety of platforms with the concept **WORA** — “**write once, run anywhere**”. Although languages like C++ compile its source code to match only a specific platform and run natively on its OS and hardware, Java source codes are compiled into an intermediate state called **bytecode** (i.e. a **.class** file) using the Java Compiler (**javac**) which comes inbuilt with **JDK**. This bytecode is in **hexadecimal** format with **opcode-operand** lines and **JVM** can interpret these instructions (without further recompilations) into native machine language which can be understood by the OS and underlying hardware platform. Therefore, bytecode acts as a **platform-independent** intermediary state which is **portable** among any JVM regardless of underlying OS and hardware architecture. However, since JVMs are developed to run and communicate with the underlying hardware & OS structure, we need to select the appropriate JVM version for our **OS version** (Windows, Linux, Mac) and **processor architecture** (x86, x64).

Java的主要力量是，被设计为跨平台运行，概念WORA-“一次编写到处运行”。尽管C++语言编译它的源码来匹配一个特定的平台，在它的系统和硬件上本地运行，Java源码被编译成一个中间状态叫字节码(一个.class文件)使用java JDK自建编译器(javac)。字节码16进制格式，包含操作码-操作对象，然后JVM可以理解这些命令(还没有更甚重编译)当做本地机器的硬件平台的OS理解的语言。所以，字节码扮演一个平台依赖居间方，可以轻便穿梭JVM无关底层系统和硬件架构。然而， 自从JVM发展为运行和沟通底层硬件和系统架构，我们需要为我们系统（Windows,Linux,Mac）和进程架构(x86,x64)选择恰当的JVM版本.

> Most of us know the above story of Java and the problem here is that the most important component of this process — the JVM is taught to us as a black box which can magically interpret bytecode and perform many run-time activities like **JIT** (Just-in-time) compilation & **GC** (Garbage Collection) during the program execution. In the next sections, let’s reveal how JVM works.

我们很多人知道上面Java的故事，这里最重要的组件处理是-JVM教会我们作为一个黑盒来处理它，他可以很魔幻的翻译字节码执行很多运行时活动，像程序执行期间的JIT(Just-in-Time) 编译器 和 GC(垃圾收集)。下一章节，我们展示JVM如何工作。

### JVM架构

> JVM is only a specification, and its implementation is different from vendor to vendor. For now, let’s understand the commonly-accepted architecture of JVM as defined in the specification.

JVM仅仅是一个规范，被不同的供应商来实现。让我们来理解规范中广泛接受的JVM架构。

![image-20221106142908072](http://rkv59lj1r.hb-bkt.clouddn.com/img/20221106142909.png)



#### 1. Class Loader Subsystem

> The **JVM resides on the RAM**. During execution, using the Class Loader subsystem, the class files are brought on to the RAM. This is called Java’s **dynamic class loading** functionality. It loads, links, and initializes the class file (.class) when it refers to a class for the first time at runtime (not compile time).

JVM存在于RAM。 执行期间，使用类加载子系统， 把class文件放进内存。这叫做Java动态类加载功能。它在运行时第一次引用类时候(而不是编译时)加载，连接初始化类文件(.class) 。

> ## 1.1) Loading
>
> Loading compiled classes (.class files) into memory is the major task of Class Loader. Usually, the class loading process starts from loading the main class (i.e. class with `static main()` method declaration). All the subsequent class loading attempts are done according to the class references in the already-running classes as mentioned in the following cases:
>
> - When bytecode make a static reference to a class (e.g. `System.out`)
> - When bytecode create a class object (e.g. `Person person = new Person("John")`)
>
> There are 3 types of class loaders (connected with inheritance property) and they follow 4 major principles.

加载编译过的类(.class文件)进内存是类加载器的主要任务。通常，类加载程序从加载main 类开始(如 class 有 static main() 方法声明)。 所有后续的类加载尝试都是根据以下情况所述已运行类中的引用进行的：

* 当字节码静态引用一个 class (如system.out)
* 当字节码创建一个类对象( Person person = new Person("John"))

3中类型的类加载器(通过继承来连接)，他们遵从4个主要原则：

> ***1.1.1) Visibility Principle\***
>
> This principle states that Child Class Loader can see the class loaded by Parent Class Loader, but a Parent Class Loader cannot find the class loaded by Child Class Loader.

1. 可见性原则

   这个原则声明子类加载器可以看到父类加载器加载的类，但是一个父类加载器不能看到子类加载器加载的类

> ***1.1.2) Uniqueness Principle\***
>
> This principle states that a class loaded by parent should not be loaded by Child Class Loader again and ensure that duplicate class loading does not occur.

2. 唯一性原则

   这个原则表明一个类被父类加载器加载后，不应该再被子类加载器加载一遍，确保不会重复加载类的事情发生。

> ***1.1.3) Delegation Hierarchy Principle\***
>
> In order to satisfy above 2 principles, JVM follows a hierarchy of delegation to choose the class loader for each class loading request. Here, starting from the lowest child level, Application Class Loader delegates the received class loading request to Extension Class Loader and then Extension Class Loader delegates the request to Bootstrap Class Loader. If the requested class found in Bootstrap path, the class is loaded. Otherwise the request again transfers back to Extension Class Loader level to find the class from Extension path or custom-specified path. If it also fails, the request comes back to Application Class Loader to find the class from System class path and if Application Class Loader also fails to load the requested class, then we get the run time exception — `java.lang.ClassNotFoundException` .

3. 委托等级原则

   为了适应以上两个原则， JVM当每个类加载请求时候，遵从一个等级委派来选择类加载器。这里，从最低层子级别开始，Application 类加载器委派收到的类加载请求给 Extension 类加载器， 然后Extension类加载器委派请求给Bootstrap类加载器。如果请求的类在Bootstrap路径中发现，类就被加载。要不然请求会被再次转移回Extension类加载器级别，去从Extension 路径或者自定义路径找需要的类。如果也失败了，请求会回到Application 类加载器，去从系统路径找需要的类，如果Application类加载器也请求失败，我们就获得了运行时异常 --- java.lang.ClassNotFoundException.

> ***1.1.4) No Unloading Principle\***
>
> Even though a Class Loader can load a class, it cannot unload a loaded class. Instead of unloading, the current class loader can be deleted, and a new class loader can be created.

4. 不卸载原则

   尽管一个类加载器可以加载一个类， 但是它不会卸载一个加载过的类。不是卸载，当前类加载器可以被删除，一个新的类加载器可以被创建。

   ![image-20221106195900722](http://rkv59lj1r.hb-bkt.clouddn.com/img/20221106195901.png)

> - **Bootstrap Class Loader** loads standard JDK classes from rt.jar such as core Java API classes present in the bootstrap path — $JAVA_HOME/jre/lib directory (e.g. java.lang.* package classes). It is implemented in native languages like C/C++ and acts as parent of all class loaders in Java.
> - **Extension Class Loader** delegates class loading request to its parent, Bootstrap and if unsuccessful, loads classes from the extensions directories (e.g. security extension functions) in extension path — $JAVA_HOME/jre/lib/ext or any other directory specified by the java.ext.dirs system property. This Class Loader is implemented in Java by the sun.misc.Launcher$ExtClassLoader class.
> - **System/Application Class Loader** loads application specific classes from system class path, that can be set while invoking a program using -cp or -classpath command line options. It internally uses Environment Variable which mapped to java.class.path. This Class Loader is implemented in Java by the sun.misc.Launcher$AppClassLoader class.

* Bootstrap Class Loader 从 rt.jar 加载JDK的类 , 例如核心Java API 类呈现在bootstrap路径 --- $JAVA_HOME/jre/lib 目录 (也就是java.lang.* 包的类) 。由本地语言如 C/C++ 实现，在Java中作为所有类加载器的父类。
* Extension Class Loader 委派类加载请求给它的父亲， Boostrap , 如果没有成功，加载类从 extensions 目录 (例如安全扩展方法) 路径是 -- $JAVA_HOME/lib/ext 或者任何一个java.ext.dirs 系统属性明确的目录。这个类加载器用Java实现。 通过 sun.misc.Lanucher$ExClassLoader Class
* System/Application Class Loader  从系统类路径加载应用程序特定的类，可以在使用-CP或-ClassPath命令行选项调用程序时设置这些类。它内部使用映射到java.class.path的环境变量。这个类加载器用Java实现，sun.misc.Launcher$AppClassLoader 类。

> *NOTE: Apart from the 3 major Class Loaders discussed above, a programmer can directly create a* ***User-defined Class Loader\*** *on the code itself. This guarantees the independence of applications through class loader delegation model. This approach is used in web application servers like Tomcat to make web apps and enterprise solutions run independently.*

注意，上面讨论的3个主要的类加载器，，一个程序员可以直接用代码自己创建一个用户定义的类加载器。这个保证了应用从类加载器委派模型的独立性。这个方法用来再累Tomcat应用服务器来使 web应用和企业解决方案独立运行。

> Each Class Loader has its **namespace** that stores the loaded classes. When a Class Loader loads a class, it searches the class based on **FQCN** (**Fully Qualified Class Name 完整类名**) stored in the namespace to check whether or not the class has been already loaded. Even if the class has an identical FQCN but a different namespace, it is regarded as a different class. A different namespace means that the class has been loaded by another Class Loader.

每个类加载器有自己的命名空间， 储存已加载的类。当一个类加载器加载一个类，他基于FQCN(Fully Qualified Class Name)来查找储存在命名空间的类，检查是否被加载过。尽管类有一个完整类名标志，但是不同命名空间，它被当作一个不同的类。一个不同的命名空间意味着类类被其他加载器加载了。



