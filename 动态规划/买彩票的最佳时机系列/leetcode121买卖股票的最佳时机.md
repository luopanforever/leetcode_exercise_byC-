## [121. 买卖股票的最佳时机 - 力扣（Leetcode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**提示：**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

**题解思路：**

回溯超时了。。

```c++
class Solution {
public:
    vector<int> path;
    int result = 0;
    void backtracking(vector<int>& princes, int startIndex){
        if(path.size() == 2){
            int val = path[1] - path[0];
            result = result >= val? result: val; 
        }

        for(int i = startIndex; i < princes.size(); i++){
            path.push_back(princes[i]);
            backtracking(princes, i + 1);
            path.pop_back();
        }
    }
    int maxProfit(vector<int>& prices) {
        backtracking(prices, 0);
        return result;
    }
};
```

**动态规划:**

- `dp`数组的含义

  - `dp`数组有两个维度，`i`表示第几天，`j`表示持有股票和不持有该天股票所得的最大现金

  - `dp[i][0]`代表第`i`天持有该股票所得的最多现金
  - `dp[i][1]`代表第`i`天不持有该股票所得的最多现金

- 确定递推公式
  - 对于`dp[i][0]`，则有两个方向可以**考虑**
    - 一个是`dp[i - 1][0]`，一个是`-prices[i]`，他俩取最大值
  - 对于`dp[i][1]`，也有两个方向可以**考虑**
    - 一个是`dp[i - 1][1]`，一个是`prices[i] + dp[i- 1][1]`，他俩起最大值，后者代表卖出股票所得现金就是按照今天股票佳价格卖出后所得现金
- `dp`数组的初始化
  - 一开始持有的现金是`0`，所以`dp[0][0]`初始化为`-prices[0]`
  - `dp[0][1]`的意义是不持有该天彩票所得现金所得的最大现金，则`dp[0][1] = 0`

- 确定遍历顺序，`i`都是由`i - 1`推导出来的，所以由前到后

**完整代码**

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<vector<int>> dp(prices.size() + 1, vector<int>(2, 0));
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for(int i = 1; i < prices.size(); i++){
            dp[i][0] = max(dp[i - 1][0], - prices[i]);
            dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
        }
        return dp[prices.size() - 1][1];
    }
};
```

