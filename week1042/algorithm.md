## ARTS - Algorithm -  [119. 杨辉三角 II](https://leetcode.cn/problems/pascals-triangle-ii/description/)

119. 杨辉三角 II

给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

### 分析

根据上一题，返回前n层的杨辉三角，我们已经有逻辑了，所以直接使用就能用，代码如下：

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        return getList(rowIndex + 1).get(rowIndex);

    }

    /**
     * 返回前n行杨辉三角
     * @param n
     */
    public List<List<Integer>> getList(int n) {

        List<List<Integer>> list = new ArrayList<>();

        for (int i = 0; i < n; i++) {

            List<Integer> layer = new ArrayList<>();

            for (int j = 0; j < i + 1; j++) {

                if (j == 0 || j == i) {
                    layer.add(1);
                } else  {
                    layer.add(list.get(i - 1).get(j - 1) + list.get(i - 1).get(j));
                }

            }

            list.add(layer);

        }

        return list;


    }
}
```



提交通过

![image-20230128152649719](http://qiniu.honeywen.com/img/image-20230128152649719.png)

**进阶：**

你可以优化你的算法到 `O(rowIndex)` 空间复杂度吗？

我们开始优化代码，我们目前的逻辑是，需要第n层，就把1-n层都生成一遍，然后取出来第n层返回，是否有办法跳过前面的n-1层，直接生成第n层？ 就是转换成一个数学问题，找到层数，与数列的对应关系， 这应该是最终的解决方案，如果暂时想不到，就考虑在目前基础上优化。



首先优化空间，我们只需要上一层的数列就够了，不用把每一组都存下来，于是首先改造成：

```java
public List<Integer> getList(int rowIndex) {

        List<Integer> prev = new ArrayList<>();

        for (int i = 0; i <= rowIndex; i++) {

            List<Integer> list = new ArrayList<>();

            for (int j = 0; j < i + 1; j++) {

                if (j == 0 ||  j== i ) {
                    list.add(1);
                } else {
                    list.add(prev.get(j - 1) + prev.get(j));
                }
                
            }
            prev = list;

        }


        return prev;
    }
```





