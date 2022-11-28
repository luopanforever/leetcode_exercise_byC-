## [309. 最佳买卖股票时机含冷冻期 - 力扣（Leetcode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

给定一个整数数组`prices`，其中第 `prices[i]` 表示第 `*i*` 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**示例 2:**

```
输入: prices = [1]
输出: 0
```

**提示：**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

#### **题解思路：**

与2的区别就在于多了一个冷却时间的条件，代码区别就在于递推公式。

`dp`数组的含义还是一样`dp[i][0]`代表持有股票获得的最大价值，`dp[i][1]`代表不持有股票获得的最大价值

**`122`的递推公式：**

```c++
dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
```

- `dp[i][0]`：有两个方向，一个是照着前一天持有彩票来办，另一个是**昨天**不持有股票的所得现金 减去 今天的股票价格
- `dp[i][1]`：就是照着前一天的不持有彩票来办，或者是卖出当天的彩票来办

**本题的递推公式：**

```c++
dp[i][0] = max(dp[i - 1][0], dp[i - 2][1] - prices[i]);
dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
```

- `dp[i][0]`：有两个方向，一个是照着前一天持有彩票来办，另一个就是与`leetcode122`的区别，该题有一个冷却期，就是卖出彩票后要等一天才能买彩票，所以第二个方向是**前天**不持有股票的所得现金 减去 今天的股票价格

- `dp[i][1]`：就是照着前一天的不持有彩票来办，或者是卖出当天的彩票来办

**`dp`数组的初始化**

由于递推公式中有`i - 2`这一条件，则`dp`数组的`[1,0]`和`[1,1]`也需要被初始化

```c++
dp[0][0] = -prices[0];
dp[0][1] = 0;
dp[1][0] = max(dp[0][0], -prices[1]);
dp[1][1] = max(dp[0][1], prices[1] - prices[0]);
```

其他都与`leetcode122`相同

#### **完整代码**

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 1) return 0;
        vector<vector<int>> dp(prices.size(), vector<int>(2, 0));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        dp[1][0] = max(dp[0][0], -prices[1]);
        dp[1][1] = max(dp[0][1], prices[1] - prices[0]); 
        for(int i = 2; i < prices.size(); i++){
            dp[i][0] = max(dp[i - 1][0], dp[i - 2][1] - prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);   
        }
        return dp[prices.size() - 1][1] > 0 ? dp[prices.size() - 1][1]: 0;
    }
};
```



