## Algorithm - [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/description/)

给你一个整数数组 `nums` 。如果任一值在数组中出现 **至少两次** ，返回 `true` ；如果数组中每个元素互不相同，返回 `false` 。

### 分析

这个是简单类别的题目。要判断某个值在数组中至少出现两次，也就是只要判断数组中有重复的元素就行。还是老样子，从你想到最能解决问题的方案入手，不用考虑性能空间， 首先能想到，利用Map的key不重复特性来处理， 遍历数组，从Map中取数，取到就说明有重复的，没有的话就继续。

代码如下：

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>(nums.length);

        for (int num : nums) {

            Integer a = map.get(num);

            if (a == null) {
                map.put(num, 1);

            } else {

                return true;
            }

        }


        return false;

    }
}
```



测试提交后，能通过：

![image-20221227103319069](https://tva1.sinaimg.cn/large/008vxvgGly1h9i5xy2jhfj31g20b8gmd.jpg)

### 优化

然后第二步考虑优化的事， 我们是否能让代码执行更快，使用更少的空间？看题目， 是一堆int数， 并且只判断是否重复就行了， 我想到了位操作中的异或操作： 相同为0，不同为1的特点。那么异或操作能否在这里使用呢？再想了想，还是没法用的，因为异或适合找存在不重复的数字，这样所有元素异或下来会大于0.翻看题解，发现也没有用位操作的，所以这种题就用一般解法吧。

看了HashMap ，其实我们不需要后边的Value, 只需要前面的Key 就够了，所以使用Set就行了：



```java
public boolean containsDuplicate(int[] nums) {

        Set<Integer> set = new HashSet<>();
        for (int num : nums) {

            if (!set.add(num)) {
                return true;
            }

        }


        return false;
    }
```



提交后结果也是可以的：

![image-20221227110635798](https://tva1.sinaimg.cn/large/008vxvgGly1h9i6whgchzj31hc0ci3zi.jpg)



然后看了其他题解， 发现通过Arrays.stream 的distinct方法，可以一行写出来：

```java
public boolean containsDuplicate(int[] nums) {

        return Arrays.stream(nums).distinct().count() < nums.length;

    }
```



只是时间较长



![image-20221227110955230](https://tva1.sinaimg.cn/large/008vxvgGly1h9i6zy7fopj31iq0b6gml.jpg)
