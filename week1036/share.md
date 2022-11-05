## ARTS - Share 补2019.3.13

第2条：使用私有构造函数强化singleton属性

singleton是指这样的类，它只能实例化一次，singleton通常被用来代表那些本质上具有唯一性的系统组件。

```java
public class Elvis {
  public static final Elvis INSTANCE = new Elvis();
  private Elvis() {}
}
```

私有构造函数仅被调用一次，用来实例化公有的平台final 域Elvis.INSTANCE. 

第二种方法是提供了一个公有的静态工厂方法，而不是公有的静态final 域

```java
public class Elvis {
  private static final Elvis INSTANCE = new Elvis();
  private Elvis() {}
  public static Elvis getInstance() {
    return INSTANCE;
  }
}
```

所有对于静态方法 Elvis.getInstance() 的调用，都会返回同一个对象引用，所以不会有别的Elvis实例被创建。

第一种方法的主要好处在于组内类的成员声明很清楚的表名了这个类是一个singleton,公有的静态域是final的，所以该域将总是包含相同的对象引用，第一中方法可能在性能上稍微领先，但是第二种方法中，一个优秀的JVM实现应该能够通过将静态工厂方法的调用内联化（inlining）， 来消除这种差别。

第二种方法的主要好处在于，它提供了灵活性：在不改变API的前提下，允许我们改变想法，把该类做成singleton，或者不做成singleton, singleton静态工厂方法返回该类的唯一实例，但是，它也很容易被修改，比如说为每一个调用该方法的线程返回一个唯一的实例。

为了使一个singleton类编程可序列话的，仅仅声明 implements Serializable 是不够的，为了维护singleton性，必须也要提供一个 readResolve 方法，否则的话，一个序列话的实例在每次反序列化时候，都会导致创建一个新的实例。

```java
private Object readResolve() throws ObjectsStreamException {
	return INSTANCE;
}
```

