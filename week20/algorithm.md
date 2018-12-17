## ARTS - Alogirthm 补11.19
### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/description/)

### 题目
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例 1:

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```
示例 2:

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

### 分析

先解决，再优化。笨方法，先跑起来再说。
类似冒泡排序，对每个元素进行比较，找出最大差额。代码如下：

```
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length < 2) {
            return 0;
        }

        int len = prices.length;

        int max = prices[len - 1] - prices[0];
        for (int i = 0; i < len - 1; i++) {
            for (int j = i + 1; j < len; j++) {

                if (prices[j] - prices[i] > max) {
                    max = prices[j] - prices[i];
                }
            }
        }


        return max < 0 ? 0 : max;
    }
}
```


### 代码
看了答案后，发现其实可以一次循环就能做到的，让左游标在值不符合时候，往右移动代码如下：


```
public int maxProfit(int[] prices) {
        int low = 0, high = 0, max = 0, tmp = 0;

        for (int i = 0; i < prices.length; i++) {

            high = i;
            tmp = prices[high] - prices[low];
            if (tmp <= 0) {
                low = i;
            }
            if (tmp > max) {
                max = tmp;
            }

        }
        return max;

    }
```