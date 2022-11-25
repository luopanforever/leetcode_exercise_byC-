## [322. 零钱兑换 - 力扣（Leetcode）](https://leetcode.cn/problems/coin-change/description/)

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

 

**提示：**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

#### **题解思路：**

**关键字**

动归、完全背包（相同硬币可以取多次）

- 物品数量 `coins.size()`

- 背包大小 `amount`

- 物品重量 ` coxins[i]`

- 物品价值` 1  `

**`dp`数组的含义**

- `dp[j]`：装满j大小的背包所需的最小物品数量

**初始化**

- 不是求方法数  全部初始化为极值， 由于是求最小所以全部初始化为`INT_MAX`即可
- `dp[0] = 0 `因为`dp`数组的含义就是这样  装满大小为`0`的背包需要`0`个物品

**递推公式**

不是求方法个数的基本都是这样的

`dp[j] = min(dp[j], dp[j - coins[i]] + 1)`

#### **完整代码：**

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, 0);
        for(int i = 1; i <= amount; i++){
            dp[i] = INT_MAX;
        }
        for(int i = 0; i < coins.size(); i++){
            for(int j = coins[i]; j <= amount; j++){
                if(dp[j - coins[i]] != INT_MAX){
                    dp[j] = min(dp[j], dp[j - coins[i]] + 1);
                }
            }
        }
        
        return dp[amount] == INT_MAX?-1:dp[amount];
    }
};
```

