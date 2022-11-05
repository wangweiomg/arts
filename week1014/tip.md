## ARTS - Tip
## Java和JavaScript 中的split

我们都知道split是字符串分割的函数，平时使用频率并不低，本人在使用时候碰到一个情况：

### 事例

现在有一个字符串 “31+” ，需要获得前面的数字"31"，本人就想到了使用split(str)[0] 的方法，于是在javascript的写法就是：

```
let s = "31+";
let result = s.split('+')[0];
console.log(result); // output 31

```

这样写是没问题的， 然后在Java中这么写，情况如下：

```
String s = "31+";
String result = s.split("+")[0];
System.out.println(result);

```

原本以为会输出 “31”， 但是结果是编译错误：

```
java.util.regex.PatternSyntaxException: Dangling meta character '+' near index 0
+
^
```

### 分析

String 的split 方法接受的是一个正则表达式，"+" 正好是正则的一个关键词，所以如果使用需要转义， 改成 “\\+” 就没问题了。

但是，既然会有编译提示，本人为什么还是碰到了报错呢？因为我吧 “+” 写在了一个常量里，

```
private static String s = "+";
    
    @org.junit.Test
    public void test5() {

        String s = "31+";
        System.out.println(s.split(s));
       

    }

```

这样是会骗过编译器， 在实际运行时就会报错了。 谨记。