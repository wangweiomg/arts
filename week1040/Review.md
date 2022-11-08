## Review - [Java类加载机制](https://www.baeldung.com/java-classloaders)

> ## **1. Introduction to Class Loaders**
>
> Class loaders are responsible for **loading Java classes dynamically to the JVM** **(Java Virtual Machine) during runtime.** They're also part of the JRE (Java Runtime Environment). Therefore, the JVM doesn't need to know about the underlying files or file systems in order to run Java programs thanks to class loaders.
>
> Furthermore, these Java classes aren't loaded into memory all at once, but rather when they're required by an application. This is where class loaders come into the picture. They're responsible for loading classes into memory.
>
> In this tutorial, we'll talk about different types of built-in class loaders and how they work. Then we'll introduce our own custom implementation.

1. 介绍类加载器

   类加载器是运行时用来动态加载Java类到JVM（Java虚拟机）。他们也是JRE（Java 运行时环境）的一部分。所以，由于有了类加载器，JVM为了运行Java程序就不需要知道底层的文件或者文件系统。

   更深的是，这些Java类并不是一次全部加载到内存，而是当应用请求时候才加载。这是类加载器发挥作用的地方。他们职责是加载类到内存。

   这个章节，我们讨论集中不同的内建类加载器是如何工作的。之后我们会介绍我们自定义实现。

   ## **2. Types of Built-in Class Loaders**

   Let's start by learning how we can load different classes using various class loaders:

   ```java
   
   
   import com.sun.javafx.util.Logging;
   
   import java.util.ArrayList;
   
   public class ClassLoaderTest {
   
       public void printClassLoaders() throws ClassNotFoundException {
   
           System.out.println("Classloader of this class:"
                   + ClassLoaderTest.class.getClassLoader());
   
           System.out.println("Classloader of Logging:"
                   + Logging.class.getClassLoader());
   
           System.out.println("Classloader of ArrayList:"
                   + ArrayList.class.getClassLoader());
       }
   
       public static void main(String[] args) throws ClassNotFoundException {
           ClassLoaderTest loader = new ClassLoaderTest();
           loader.printClassLoaders();
   
       }
   }
   
   ```

   > As we can see, there are three different class loaders here: application, extension, and bootstrap (displayed as *null*).
   >
   > The application class loader loads the class where the example method is contained. **An application or system class loader loads our own files in the classpath.**
   >
   > Next, the extension class loader loads the *Logging* class. **Extension class loaders load classes that are an extension of the standard core Java classes.**
   >
   > Finally, the bootstrap class loader loads the *ArrayList* class. **A bootstrap or primordial class loader is the parent of all the others.**
   >
   > However, we can see that for the *ArrayList,* it displays *null* in the output. **This is because the bootstrap class loader is written in native code, not Java, so it doesn't show up as a Java class.** As a result, the behavior of the bootstrap class loader will differ across JVMs.

   我们看到，有三种不同的类加载器： application, extension, 和bootstrap(这里显示null).

   application 类加载器加载包含示例方法的类。一个application 或者system 类加载器加载我们自己的在classpath的文件。

   Extensioni 类加载器加载 Logging 类。Extension类加载器加载标准Java库的类之外的那些扩展类。

   Bootstrap类加再起加载ArrayList 类。一个bootstrap或者primordial类加载器是所有其他加载器的父亲。

   然而，我们看到ArrayList ， 显示null. 这是应为bootstrap 类加载器使用本地代码写的，不是Java，所以不显示Java class. 因此，bootstrap 类加载器在不同JVM中表现是不同的。

   

   >  Now let's discuss each of these class loaders in more detail. 
   >
   > ### **2.1. Bootstrap Class Loader**
   >
   > Java classes are loaded by an instance of *java.lang.ClassLoader*. However, class loaders are classes themselves. So the question is, who loads the *java.lang.ClassLoader* itself*?*
   >
   > This is where the bootstrap or primordial class loader comes into play.
   >
   > It's mainly responsible for loading JDK internal classes, typically *rt.jar* and other core libraries located in the *$JAVA_HOME/jre/lib* directory. Additionally, the **Bootstrap class loader serves as the parent of all the other \*ClassLoader\* instances**.
   >
   > **This bootstrap class loader is part of the core JVM and is written in native code,** as pointed out in the above example. Different platforms might have different implementations of this particular class loader.

   Java类被一个java.lang.ClassLoader的实例加载。然而，类加载器是类自己，所以问题是，谁来加载 java.lang.ClassLoader 自身？

   这就是bootstrap ，primoridial 类加载器发挥作用的地方。

   主要职责是加载JDK内部的类，专门的 rt.jar 和其他核心库，在路径 $JAVA_HOME/jre/lib 目录

   另外，bootstrap 类加载器服务作为所有其他 类加载器实例的父类.

   这个bootstrap 类加载器是本地代码写的核心JVM的一部分， 如上个例子所述。不同平台可能对这个类加载器有不同的实现。

   我的lib:

   ![image-20221108185208143](/Users/weiwang/Library/Application Support/typora-user-images/image-20221108185208143.png)

   > ### **2.2. Extension Class Loader**
   >
   > The **extension class loader is a child of the bootstrap class loader, and takes care of loading the extensions of the standard core Java classes** so that they're available to all applications running on the platform.
   >
   > The extension class loader loads from the JDK extensions directory, usually the *$JAVA_HOME/lib/ext* directory, or any other directory mentioned in the *java.ext.dirs* system property.

   Extension 类加载器是bootstrap的孩子，关注加载Java标准核心类的扩展，所以就对按所有运行在平台上的 应用可用。

   Extension 类加载器从 JDK 扩展目录加载， 通常是 $JAVA_HOME/lib/ext 目录 ， 或者其他java.ext.dirs系统属性提及到的其他目录

   我的ext:

   ![image-20221108185258499](https://tva1.sinaimg.cn/large/008vxvgGly1h7xx0nsdqij32ge04sjsa.jpg)

   > ### **2.3. System Class Loader**
   >
   > The system or application class loader, on the other hand, takes care of loading all the application level classes into the JVM. **It loads files found in the classpath environment variable, \*-classpath,\* or \*-cp\* command line option**. It's also a child of the extensions class loader.

   system或application类加载器，换句话说，关心加载所有的应用级别类到JVM。 加载在classpath 环境变量 *-classpath,* 或者 \*-cp\* 命令行指定的路径。也是extension 类加载器的孩子。

   > ## **3. How Do Class Loaders Work?**
   >
   > Class loaders are part of the Java Runtime Environment. When the JVM requests a class, the class loader tries to locate the class and load the class definition into the runtime using the fully qualified class name.
   >
   > The ***java.lang.ClassLoader.loadClass()\* method is responsible for loading the class definition into runtime**. It tries to load the class based on a fully qualified name.
   >
   > If the class isn't already loaded, it delegates the request to the parent class loader. This process happens recursively.
   >
   > Eventually, if the parent class loader doesn’t find the class, then the child class will call the *java.net.URLClassLoader.findClass()* method to look for classes in the file system itself. 
   >
   > If the last child class loader isn't able to load the class either, it throws [*java.lang.NoClassDefFoundError* or *java.lang.ClassNotFoundException.*](https://www.baeldung.com/java-classnotfoundexception-and-noclassdeffounderror)

   类加载器是Java运行时环境的 一部分。当JVM请求一个类， 类加载器尝试定位类，使用全类名加载类定义到运行时。

   Java.lang.ClassLoader.loadClass() 方法职责是加载类定义到运行时。它基于一个全路径名加载类。

   如果类没有加载，就委派请求给父亲加载器。这个过程是递归的。

   如果父亲加载器也没有发现类，然后子类将会调用java.net.URLClassLoader.findClass() 方法寻找类。

   如果最后子类加载器也没有加载到类，就会抛出java.lang.NoClassDefFoundError 或者java.lang.ClassNotFoundException.

   >Let's look at an example of the output when *ClassNotFoundException* is thrown:

   >```java
   >java.lang.ClassNotFoundException: com.baeldung.classloader.SampleClassLoader    
   >    at java.net.URLClassLoader.findClass(URLClassLoader.java:381)    
   >    at java.lang.ClassLoader.loadClass(ClassLoader.java:424)    
   >    at java.lang.ClassLoader.loadClass(ClassLoader.java:357)    
   >    at java.lang.Class.forName0(Native Method)    
   >    at java.lang.Class.forName(Class.java:348)
   >```

看一个例子。



> If we go through the sequence of events right from calling *java.lang.Class.forName()*, we can see that it first tries to load the class through the parent class loader, and then *java.net.URLClassLoader.findClass()* to look for the class itself.
>
> When it still doesn't find the class, it throws a *ClassNotFoundException.*
>
> Now let's examine three important features of class loaders.

如果我们通过序列事件调用java.lang.Class.forName()， 我们可以看到首先尝试加载class 通过父亲类加载器，之后它自己尝试java.net.URLClassLoader.findClass() 。
