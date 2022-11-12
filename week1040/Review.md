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

如果我们通过序列事件调用java.lang.Class.forName()， 我们可以看到首先尝试加载class 通过父亲类加载器，之后它自己尝试java.net.URLClassLoader.findClass() 来找类。

当也找不到Class，就会抛出ClassNotFoundExceptioni. 

现在来考察类加载器的三个重要特性

> ### **3.1. Delegation Model**
>
> Class loaders follow the delegation model, where **on request to find a class or resource, a \*ClassLoader\* instance will delegate the search of the class or resource to the parent class loader**.
>
> Let's say we have a request to load an application class into the JVM. The system class loader first delegates the loading of that class to its parent extension class loader, which in turn delegates it to the bootstrap class loader.
>
> Only if the bootstrap and then the extension class loader are unsuccessful in loading the class, the system class loader tries to load the class itself.

委派模型

类加载器遵从的模型，当请求找一个类或者资源， 一个 类加载器实例将会委派给父加载器来找类或资源。

我们有一个请求需要加载应用class到JVM。system类加载器委派它的父亲 extension 加载器来加载类，然后它委托给bootstrap加载器。

当仅当bootstrap 和之后的 extension类加载没有加载到类，sysytem类加载器才会自己尝试加载类。

> ### **3.2. Unique Classes**
>
> As a consequence of the delegation model, it's easy to ensure **unique classes, as we always try to delegate upwards**.
>
> If the parent class loader isn't able to find the class, only then will the current instance attempt to do so itself.

唯一类。

作为一个委派模型结果，很容易确保唯一类，因为总是尝试向上委派。

如果父加载器没找到类，当前实例才会自己去加载。

> ### **3.3. Visibility**
>
> In addition, **children class loaders are visible to classes loaded by their parent class loaders**.
>
> For instance, classes loaded by the system class loader have visibility into classes loaded by the extension and bootstrap class loaders, but not vice-versa.
>
> To illustrate this, if Class A is loaded by the application class loader, and class B is loaded by the extensions class loader, then both A and B classes are visible as far as other classes loaded by the application class loader are concerned.
>
> Class B, however, is the only class visible to other classes loaded by the extension class loader.

可见性。

更多的是，子加载器对父加载器加载的类是可见的。

例如，被system加载器加载的类对 extension和bootstrap加载器加载的类具有可见性，反之不然。

为了解释这一点，如果 Class A 被 application 加载器加载，class B 被extensions 加载，那么 A和B类都对 application 加载器加载的类可见。

Class B, 却只能被extension 加载的类可见。



> ## **4. Custom ClassLoader**
>
> The built-in class loader is sufficient for most cases where the files are already in the file system.
>
> However, in scenarios where we need to load classes out of the local hard drive or a network, we may need to make use of custom class loaders.
>
> In this section, we'll cover some other use cases for custom class loaders and demonstrate how to create one.

自定义加载器

内建加载器对大多数已经在文件系统中的文件是足够了。

然而，我们还需要加载本地硬件或者网络之外的的类，我们可能需要使用自定义加载器。

> ### 4.1. Custom Class Loaders Use-Cases
>
> Custom class loaders are helpful for more than just loading the class during runtime. A few use cases might include:
>
> 1. Helping to modify the existing bytecode, e.g. weaving agents
> 2. Creating classes dynamically suited to the user's needs, e.g. in JDBC, switching between different driver implementations is done through dynamic class loading.
> 3. Implementing a class versioning mechanism while loading different bytecodes for classes with the same names and packages. This can be done either through a URL class loader (load jars via URLs) or custom class loaders.
>
> Below are more concrete examples where custom class loaders might come in handy.
>
> **Browsers, for instance, use a custom class loader to load executable content from a website.** A browser can load applets from different web pages using separate class loaders. The applet viewer, which is used to run applets, contains a *ClassLoader* that accesses a website on a remote server instead of looking in the local file system.
>
> It then loads the raw bytecode files via HTTP, and turns them into classes inside the JVM. Even if these **applets have the same name, they're considered different components if loaded by different class loaders**.
>
> Now that we understand why custom class loaders are relevant, let's implement a subclass of *ClassLoader* to extend and summarise the functionality of how the JVM loads classes.

自定义类加载器用例

自定义类加载器对于不仅在运行时加载类是很有帮助的。一些用例可能包括：

1. 帮助改已经存在的字节码，例如编织代理
2. 动态创建类来适应用户所需，例如在JDBC，切换不同的驱动就是通过动态加载类来实现的。
3. 在为具有相同名称和软件包的类加载不同的字节码时，实现类版本控制机制。这可以通过URL 类加载器(通过URL来加载jar) 或者自定义类加载器。

底下是更确切的例子，自定义类加载器场景，

浏览器，实际上，使用一个定义加载器从网站上加载可执行内容。一个浏览器可以加载 applets 从不同的网页通过使用不同的类加载器。applet viewer, 用来跑applets, 包括一个类加载器操作远端服务器，而不是找本地文件系统。

之后通过HTTP加载未加工的字节码文件，在JVM内部转化成class. 尽管这些applets 有相同的名字，但是如果通过不同的类加载器加载，就被认为是不同组件。

现在我们理解了为什么自定义加载器是有意义的，我们实现一个ClassLoader的子类来扩展总结JVM是如何加载class的。

> ### 4.2. Creating Our Custom Class Loader
>
> For illustration purposes, let's say we need to load classes from a file using a custom class loader.
>
> **We need to extend the \*ClassLoader\* class and override the \*findClass()\* method:**

需要继承ClassLoader 类，重写findClass方法

```java
import java.io.*;

public class CustomClassLoader extends ClassLoader{

    @Override
    public Class<?> findClass(String name) throws ClassNotFoundException {
//        return super.findClass(name);

        byte[] b = loadClassFromFile(name);
        return defineClass(name, b, 0, b.length);
    }

    private byte[] loadClassFromFile(String fileName) {
        InputStream inputStream = getClass().getClassLoader().getResourceAsStream(fileName.replace('.', File.separatorChar) + ".class");
        byte[] buffer;
        ByteArrayOutputStream byteStream = new ByteArrayOutputStream();
        int nextValue = 0;
        try {

            while ((nextValue = inputStream.read()) != -1) {
                byteStream.write(nextValue);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        buffer = byteStream.toByteArray();
        return buffer;

    }
}

```



> In the above example, we defined a custom class loader that extends the default class loader, and loads a byte array from the specified file.

上面例子，我们定义了一个自定义类加载器继承默认类加载器，加载字节数组从特定的文件。

> ### 5. Understanding *java.lang.ClassLoader*
>
> Let's discuss a few essential methods from the *java.lang.ClassLoader* class to get a clearer picture of how it works.
>
> ### 5.1. The *loadClass()* Method

理解java.lang.ClassLoader

我们讨论一些java.lang.ClassLoader的必要方法来更清楚它是如何工作的。

```public Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {```

> This method is responsible for loading the class given a name parameter. The name parameter refers to the fully qualified class name.
>
> The Java Virtual Machine invokes the *loadClass()* method to resolve class references, setting resolve to *true*. However, it isn't always necessary to resolve a class. **If we only need to determine if the class exists or not, then the resolve parameter is set to \*false\*.**
>
> This method serves as an entry point for the class loader.
>
> We can try to understand the internal working of the *loadClass()* method from the source code of *java.lang.ClassLoader:*

这个方法职责是根据name 参数来加载类。 name参数代表符合条件的类名字。

Java虚拟机调用 loadClass() 方法来解析类引用，设置resolve为true。  然而，它并非每次都需要解析一个类。如果我们仅仅需要确定类是否存在，resolve参数设置为 false.

这个方法充当类加载器的入口点。

我们可以从 java.lang.ClassLoader的源码来理解内部工作：

```java
protected Class<?> loadClass(String name, boolean resolve)
  throws ClassNotFoundException {
    
    synchronized (getClassLoadingLock(name)) {
        // First, check if the class has already been loaded
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    c = findClass(name);
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }
```

> The default implementation of the method searches for classes in the following order:
>
> 1. Invokes the *findLoadedClass(String)* method to see if the class is already loaded.
> 2. Invokes the *loadClass(String)* method on the parent class loader.
> 3. Invoke the *findClass(String)* method to find the class.

搜索类方法的默认实现是如下顺序：

1. 调用 findLoaderClass(String) 方法可按是否已经加载
2. 调用 父加载器的 loadClass(String) 方法
3. 调用 findClass(String)来查找

> ### 5.2. The *defineClass()* Method

```java
protected final Class<?> defineClass(
  String name, byte[] b, int off, int len) throws ClassFormatError
```

> This method is responsible for the conversion of an array of bytes into an instance of a class. Before we use the class, we need to resolve it.
>
> If the data doesn't contain a valid class, it throws a *ClassFormatError.*
>
> Also, we can't override this method, since it's marked as final.

这方法只是是转换一个字节数组为一个类实例。我们用类之前，我们需要调用它。

如果数据不包括一个合法的类，就会抛出 ClassFormatError

我们不能覆盖这个方法，因为它被标记为 final.

> ### 5.3. The *findClass()* Method

```java
protected Class<?> findClass(
  String name) throws ClassNotFoundException
```

> This method finds the class with the fully qualified name as a parameter. We need to override this method in custom class loader implementations that follow the delegation model for loading classes.
>
> In addition, *loadClass()* invokes this method if the parent class loader can't find the requested class.
>
> The default implementation throws a *ClassNotFoundException* if no parent of the class loader finds the class.

这个方法根据全路径名称来找类。我们需要覆盖它在自定义类加载器实现里，遵从加载类的委派模型。

另外，loadClass 调用这方法当父类加载器没有找到请求的类时候。

默认实现抛出一个 ClassNotFoundException 如果父亲加载器没有找到类。

> ### 5.4. The *getParent()* Method

```public final ClassLoader getParent()```

> This method returns the parent class loader for delegation.
>
> Some implementations, like the one seen before in Section 2, use *null* to represent the bootstrap class loader.

这个方法返回委派的父加载器。

有些实现，使用null来代表bootstrap类加载器。

> ### 5.5. The *getResource()* Method

``` public URL getResource(String name)```

> This method tries to find a resource with the given name.
>
> It will first delegate to the parent class loader for the resource. **If the parent is \*null\*, the path of the class loader built into the virtual machine is searched.** 
>
> If that fails, then the method will invoke *findResource(String)* to find the resource. The resource name specified as an input can be relative or absolute to the classpath.
>
> It returns an URL object for reading the resource, or null if the resource can't be found or the invoker doesn't have adequate privileges to return the resource.
>
> It's important to note that Java loads resources from the classpath.
>
> Finally, **resource loading in Java is considered location-independent,** as it doesn't matter where the code is running as long as the environment is set to find the resources.

这个方法尝试根据给定名字来找资源。

首先会委派父加载器去找资源。如果 父亲是 null, 虚拟机内建的类加载器路径就找到了。

如果失败，方法会调用 findResource(String)来找资源。资源名字明确作为一个输入，可以是相对或绝对路径。

返回一个URL对象来读资源，或者资源找不到或调用者没有权限，就返回null。

留意到Java加载资源从classpath 是很重要的。

最后，Java中的资源加载被认为是位置无关的，因为只要将环境设置为查找资源，代码在哪里运行并不重要。



> ## 6. Context Classloaders
>
> In general, context class loaders provide an alternative method to the class-loading delegation scheme introduced in J2SE.
>
> Like we learned before, **classloaders in a JVM follow a hierarchical model, such that every class loader has a single parent with the exception of the bootstrap class loader.**
>
> However, sometimes when JVM core classes need to dynamically load classes or resources provided by application developers, we might encounter a problem.
>
> For example, in JNDI, the core functionality is implemented by the bootstrap classes in *rt.jar.* But these JNDI classes may load JNDI providers implemented by independent vendors (deployed in the application classpath). This scenario calls for the bootstrap class loader (parent class loader) to load a class visible to the application loader (child class loader).
>
> **J2SE delegation doesn't work here, and to get around this problem, we need to find alternative ways of class loading. This can be achieved using thread context loaders.**
>
> The *java.lang.Thread* class has a method, ***getContextClassLoader(),\* that returns the \*ContextClassLoader\* for the particular thread**. The *ContextClassLoader* is provided by the creator of the thread when loading resources and classes.
>
> If the value isn't set, then it defaults to the class loader context of the parent thread.

上下文类加载器

通常，context 类加载器提供一个可供选择的方法给 类加载委派语法 使用J2SE.

就像我们之前了解的， 类加载器器在JVM中遵从一个竖向的模型，每个类加载器有一个单独的父亲除了 bootstrap 类加载器。

然而，有时应用开发者需要JVM核心类动态加载类或者资源，我们可能会遇到问题。

例如， 在JNDI， 核心方法是在rt.jar 的bootstrap 类实现的 。但是这些JDNI 类可能加载 JDNI提供者实现通过独立的 vendors(部署在应用 类路径)。此场景调用bootstrap类加载器(父类加载器)加载 application加载器(子加载器)可见的类。

J2SE委派在这里不工作，围绕这个问题，我们需要寻找类加载的替代方法。这可以使用线程context加载器来实现。

java.lang.Thread 类有一个方法， getContextClassLoader() ， 返回 ContextClassLoader 为特定线程。 ContextClassLoader 是线程创建者提供的当加载资源和类时候。

如果值没有设置，默认使用付线程的类加载器上下文。

> ## **7. Conclusion**
>
> Class loaders are essential to execute a Java program. In this article, we provided a good introduction to them.
>
> We discussed the different types of class loaders, namely Bootstrap, Extensions, and System class loaders. Bootstrap serves as a parent for all of them, and is responsible for loading the JDK internal classes. Extensions and system, on the other hand, load classes from the Java extensions directory and classpath, respectively.
>
> We also learned how class loaders work and examined some features, such as delegation, visibility, and uniqueness. Then we briefly explained how to create a custom class loader. Finally, we provided an introduction to Context class loaders.
>
> As always, the source code for these examples can be found [over on GitHub](https://github.com/eugenp/tutorials/tree/master/core-java-modules/core-java-jvm).

总结

类加载器是执行Java程序的要点。这篇文章，我们提供了好的介绍。

我们讨论了不同类型的类加载器，叫 Bootstrap, Extension,和system 类加载器。 Bootstrap服务为所有的父类加载器，职责是加载JDK内部类。另一方面，扩展和系统分别从Java扩展目录和类路径加载类。

我们也学到了类加载器如何工作，检查了一些特性，例如委派，可见性和不重复行。之后解释了创建一个自定义加载器。最后，我们提供了介绍Context 类加载器。 



