## ARTS - Algorithm
## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/description/)

### 题目
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

#### 示例 1：

##### 输入： 2
##### 输出： 2
##### 解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶

####示例 2：

#####输入： 3
#####输出： 3
#####解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶

### 分析

说实话这题确实没想到是斐波那契数列，感觉没有头绪，看了答案才明白这个，希望在下次碰到同类型的题多列举几次，去发现规律。


比如

```
n = 1
1. 1 + 1

n = 2
1. 1 + 1
2. 2

n = 3
1. 1 + 1 + 1
2. 1 + 2
3. 2 + 1

n = 4
1. 1, 1, 1, 1
2. 2, 1, 1
3. 1, 2, 1
4. 1, 1, 2
5. 2, 2

```

这样就能容易些看出规律...

### 算法

```

public int climbStairs(int n) {

        if (n <= 2) {
            return n;
        }
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
    
    
    
     public int climbStairs2(int n) {
        if (n <= 1) {
            return 1;
        }
        int oneStep = 1;
        int twoStep = 1;
        int res = 0;
        for (int i = 2; i <= n; i++) {
            res = oneStep + twoStep;
            twoStep = oneStep;
            oneStep = res;

        }
        return res;

    }
```

