## ARTS - Algorithm - [191. 位1的个数 - 力扣（Leetcode）](https://leetcode.cn/problems/number-of-1-bits/description/)

### 题目

191. 位1的个数

编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为[汉明重量](https://baike.baidu.com/item/汉明重量)）。

 

**提示：**

- 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
- 在 Java 中，编译器使用[二进制补码](https://baike.baidu.com/item/二进制补码/5295284)记法来表示有符号整数。因此，在上面的 **示例 3** 中，输入表示有符号整数 `-3`。



### 分析

按位与， 每一位与1进行按位与， 是1就计数器加一，由于int 是32位整数， 就直接迭代32次。

代码：

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        for (int i = 0; i < 32; i++) {

            if ((n & 1) == 1) {
                count++;
            }
            n = n >> 1;

        }

        return count;
    }
}
```

![image-20230207124932784](http://qiniu.honeywen.com/img/20230207124933.png)



看官方题解后， 发现其实可以不用修改输入 n 的值，改造我们的代码，其实引入 1 来做位移就够了

```java
public int hammingWeight(int n) {
        int count = 0;

        for (int i = 0; i < 32; i++) {

            if ((n & (1 << i)) == 1) {
                count++;
            }

        }
        return count;
    }
```

但是在过用例时候错了：

![image-20230208215110526](http://qiniu.honeywen.com/img/20230208215112.png)

为什么会有这个错误？我们分析代码单步执行， 

i = 0,   1 << 0   是  1 

i = 1, 1 << 1 是 2 ， 这里  2 是不等于 1 的，所以没记上数，但是应该记数

i = 2 1<< 2 是  4， 这时候和 1011 按位与， 是0 ，所以不计数，

所以根据以上， 逻辑应该是 按位与后是0， 就不该记数，不是0 才该记数，修改逻辑为：

```java
public int hammingWeight(int n) {
        int count = 0;

        for (int i = 0; i < 32; i++) {

            if ((n & (1 << i)) != 0) {
 
                count++;
            }

        }
        return count;
    }
```

这样就可以了。

![image-20230208220607765](C:\Users\Welto\AppData\Roaming\Typora\typora-user-images\image-20230208220607765.png)



但是，这里使用内存却上升了， 想想确实是引入中间变量导致的， 所以最优解还是第一个。