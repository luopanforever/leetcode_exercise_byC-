## [188. 买卖股票的最佳时机 IV - 力扣（Leetcode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)

给定一个整数数组 `prices` ，它的第 `i` 个元素 `prices[i]` 是一支给定的股票在第 `i` 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 **k** 笔交易。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1：**

```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

**示例 2：**

```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```

**提示：**

- `0 <= k <= 100`
- `0 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`

#### 题解思路：

与2的区别是限定了最多只能买两次

直接上代码

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // j有五个状态
        /*
        0：什么都不做
        1：第一次买入
        2：第一次卖出
        3：第二次买入
        4：第二次卖出
        */
        vector<vector<int>> dp(prices.size(), vector<int>(5, 0));
        // 初始化
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];

        for(int i = 1; i < prices.size(); i++){
            dp[i][0] = dp[i - 1][0];  // 什么都不做
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
            dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
            dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
        }
        return dp[prices.size() - 1][4];
    }
};
```

