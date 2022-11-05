## ARTS - Algorithm 补2019.1.9
## [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/submissions/)


### 题目
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```  

示例 2:

```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

示例 3:

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

### 分析
所谓低买高卖没有手续费。我们先找买入时机，再找卖出时机，买入时机就是比后一天价格低，卖出时机就是阶段高点，也就是明天价格比今天低。那么代码如下：

```
		 
        int max = 0;
        int len = prices.length;
        boolean buy = false;
        int buyIndex = -1;
        for (int i = 0; i < len - 1; i++) {
            // 买
            if (prices[i] < prices[i + 1] && !buy) {
                buy = true;
                buyIndex = i;
            }

            // 卖
            if (prices[i] > prices[i + 1] && buy) {
                buy = false;
                max += (prices[i] - prices[buyIndex]);
            }

        }

		 // 还没有卖，就最后一天价格成交
        if (buy) {
            max += (prices[len - 1] - prices[buyIndex]);
        }

        return max;
```

审查这个代码，我们需要一个标志确定当前是否买入了股票，使用了一个布尔型来确定，使用 buyIndex记录买入的价格位置，需要卖时候使用卖价减去买入价就是利润。 其实，在拥有买入价格位置索引时候，就已经代表买入了，如果卖出就设置为负，那么久不用多一个布尔标志了。

简化后如下：

```

		int max = 0;
        int low = -1;
        int len = prices.length;
        for (int i = 0; i < len - 1; i++) {
      		// 买入
            if (prices[i] < prices[i + 1] && low < 0) {
                low = i;
            }
            // 卖出
            if (prices[i] > prices[i + 1] && low >= 0) {
                max += (prices[i] - prices[low]);
                low = -1;
            }
        }

        if (low >= 0) {
            max += (prices[len - 1] - prices[low]);
        }

        return max;

```

我们继续分析，其实我们只需要正确卖出时候计算利润就行了，其他情况不计算利润，那么代码简写如下：


### 代码

```
 	public int maxProfit(int[] prices) {
        // 赚钱就卖

        int max = 0;
        for (int i = 0, len = prices.length; i < len - 1; i++) {
            max += (Math.max(0, prices[i + 1] - prices[i]));

        }

        return max;

    }
```