## ARTS - Algorithm
## 补10.15号

## 746. 使用最小花费爬楼梯

数组的每个索引做为一个阶梯，第 i个阶梯对应着一个非负数的体力花费值 cost[i](索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

示例 1:

输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
 示例 2:

输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6


### 分析
这道题和[70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/description/) 更进一步，是要选出最小花费的那条走法。

我的思考是，首先选择走一步，还是走两步，每次选择时候就找出最小花费的那个，也是就有了如下代码：


```

public static int minCostClimbingStairs(int[] cost) {

        int len = cost.length;

        if (len == 2) {
            return cost[0] <= cost[1] ? cost[0] : cost[1];
        }

        return getResult(cost);


    }
    
    
    public static int getResult(int[] cost) {
            int res = 0;
            int len = cost.length;
            int idx = -1;

            while (idx + 2 < len) {
                int r1 = cost[idx + 1];
                int r2 = cost[idx + 2];
                if (r1 < r2) {
                    res += r1;
                    idx += 1;
                } else {
                    res += r2;
                    idx += 2;
                }

            }

            return res;

        }
```

这个代码相当于从 -1 位置出发，每次取出走一步或者走两步对应的花费，然后选择花费最小的，然后在此基础上决策下一步，贪心算法的思维。很明显这个解答是错误的，

比如对： 10, 15, 20， 这个算法首先选择是 10， 然后在15，20 中选择了15， 这样总花费是25， 其实我们知道最小花费是15.

为了解决这个问题，考虑到从后往前也算一遍，这样取得两次最小的那个，就是最小花费路径了。比如对 10，15，20， 就会选择15，然后结束。

于是代码就又变成了：

```
public static int minCostClimbingStairs(int[] cost) {

        int len = cost.length;

        if (len == 2) {
            return cost[0] <= cost[1] ? cost[0] : cost[1];
        }

        int r1 = getResult1(cost);

        int r2 = getResult2(cost);

        return r1 <= r2 ? r1 : r2;

    }

        public static int getResult2(int[] cost) {
            int res = 0;
            int idx = cost.length;


            while (idx - 2 > -1) {
                int r1 = cost[idx - 1];
                int r2 = cost[idx - 2];
                if (r1 < r2) {
                    res += r1;
                    idx -= 1;
                } else {
                    res += r2;
                    idx -= 2;
                }

            }

            return res;

        }

        public static int getResult1(int[] cost) {
            int res = 0;
            int len = cost.length;
            int idx = -1;

            while (idx + 2 < len) {
                int r1 = cost[idx + 1];
                int r2 = cost[idx + 2];
                if (r1 < r2) {
                    res += r1;
                    idx += 1;
                } else {
                    res += r2;
                    idx += 2;
                }

            }

            return res;

        }


```


在提交后，又没有通过，因为存在如下情况：

{0, 2, 3, 2}

从前往后， 会选择 0，2，2  花费 4
从后往前， 会选择 2， 2  花费 4
， 结果返回4。但是我们知道正确路径是 0， 3 ， 最小花费是3。


为什么这样的算法会出错？因为它只做到了针对当前路径的最小花费，没有考虑到全部路径的最小花费，所以算法是错的。再看了答案后，学到了正确的算法， 使用递归，直到找到最小花费。


### 代码

代码如下：

```
public static int minCostClimbingStairs2(int[] cost) {

        int[] m = new int[cost.length + 1];
        return dp(cost, m, cost.length);

    }

    private static int dp(int[] cost, int[] m, int i) {
        if (i <= 1) {
            return 0;
        }

        if (m[i] > 0) {
            return m[i];
        }
        return m[i] = Math.min(dp(cost, m, i - 1) + cost[i - 1], dp(cost, m, i - 2) + cost[i - 2]);

    }

```

这个算法每步都会有两个选择，每步都去找到这步选择的最优解，当前的最优解又依赖于下一个的最优解，这样直到走完才能找到整体最优解，于是就得出了答案。

