## ARTS - Algorithm
## [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/description/)

### 题目


实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

示例 1:

输入: 4
输出: 2
示例 2:

输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。


### 分析
这个是求开方的结果，很容易想到按顺序i找，如果存在 i 就去找 ```i*i <= x && （i+1）*(i+1)>x```的情况，这个i就是要的结果，于是代码就如下了；

```
public int mySqrt(int x) {
        if (x <= 0) {
            return 0;
        }

        for (int i = 0; i < x; i++) {
            boolean flag = i*i <= x && (i + 1) * (i + 1) > x;
            if (flag) {
                return i;
            }
        }

        return 0;

    }
```

然后提交后就失败了，因为存在 int 越界的情况，如果 i*i 大于 int最大值，那么就得不到正确的答案了，于是就把 int转为long型来处理：

```
boolean flag = (long) i * (long) i <= x && (long) (i + 1) * (long) (i + 1) > x;
```

结果通过。

### 优化
首先找出解决问题的方法，然后再考虑优化解决方法。我们发现，按顺序i +1 一个个试是比较低效的，应该优化下查找方法，使用二分查找， 而且最大值显然不可能是x, 连 x/2+1 都超不过，  所以我们可以在 0 和 x/2+1 间进行二分查找， 修改成如下方法：

```
public static int mySqrt(int x) {

        if (x <= 0) {
            return 0;
        }
        int max = x / 2 + 1;
        int min = 0;
        while (min <= max) {
            int mid = min + (max - min) / 2;
            long rs = (long) mid * (long) mid;
            if (rs == x) {
                return mid;
            }

            if (rs < x) {
                min = mid + 1;
            } else {
                max = mid - 1;
            }
        }

        return max;

    }

```
