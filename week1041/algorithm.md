## ARTS - Algorithm - [杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![image-20230104230928121](http://qiniu.honeywen.com/img/20230104230929.png)

### 分析

杨辉三角属于简单类型的题。根据题目分析，我们总结几个杨辉三角的特点：

1. 每一层数据依赖相邻他的上一层数据，
2. 第n层有n个数据
3. 每一层两边外侧是1， 也就是说，每一层的起点和终点元素是1\

根据以上特点推断代码的核心逻辑，进行第n层数据确定时候，需要知道n-1层的数据， 第n层第 i 个元素 是n-1层 第 i-1元素 和 第 i 个元素的和。 



### 代码

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> list = new ArrayList<>(numRows);
        List<Integer> one = new ArrayList<>(1);
        one.add(1);
        list.add(one);
        for (int i = 2; i <= numRows; i++) {
            // 上一层数据
            List<Integer> lastLayer = list.get(i - 1 - 1);
            // 第n层有n个数
            List<Integer> layer = new ArrayList<>(i);

            // 第1个是1， 
            layer.add(1);
            
            for (int j = 1; j < i; j++) {
                // 本层第 j 个数，是上一层第 j-1 , j 的和
                // 如果是最后一个，直接赋值1，跳出本次循环
                if (j == i - 1) {
                    layer.add(1);
                    continue;
                }
                int rs = lastLayer.get(j-1) + lastLayer.get(j);
                layer.add(rs);

            }

            list.add(layer);

        }


        return list;

    }
}
```

### 优化

先解决，再优化。 我们来看如何优化上面代码。

从我们得出的结论下手：

**第n层有n个数据**。  **每一层的起点和终点元素是1**

我们上面的代码，首先第一层特殊处理了：

```java
List<Integer> one = new ArrayList<>(1);
one.add(1);
list.add(one);
```

这一层能否放进循环里？ 我们的i表示什么？ 内循环里 j表示什么？ 

####  i 表示什么

i表示的第几层，  如果i 从0开始， 那么第  i+1 层就有  i+1个数据。

#### j 表示什么

j 表示当前行第 j 个数据。j 和 i 的关系呢?  j 只能 < i +1。

所以，核心逻辑修改为:

```java
public List<List<Integer>> generate(int numRows) {
        
        List<List<Integer>> list = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {

            List<Integer> row = new ArrayList<>();

            for (int j = 0; j < i + 1; j++) {

                if (j == 0 || j == i) {
                    row.add(1);
                } else {
                    row.add(list.get(i - 1).get(j - 1) + list.get(i - 1).get(j));
                }

            }

            list.add(row);

        }


        return list;
    }
```









