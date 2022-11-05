## ARTS - Tip 补 11.26
### 关于字符串类型和数值类型排序的bug

最近遇到了一个排序需求，于是就直接使用了 Collections.sort 方法来实现，但是没有注意元素类型，就导致排序有问题。

如下：

```
		
List<String> list = Arrays.asList("4.2, 3.3, 1, 17, 25".split(","));
System.out.println(list);
Collections.sort(list);
System.out.println(list);
    
// output
[4.2,  3.3,  1,  17,  25]
[ 1,  17,  25,  3.3, 4.2]
```

正确的做法是转成数字类型再排序

```
Collections.sort(list, Comparator.comparingDouble(o -> Double.parseDouble(o)));
// output
[ 1,  3.3, 4.2,  17,  25]

```

这是个非常基础的问题，能在这么小的问题上犯错，也是无地自容了。