### [343. 整数拆分 - 力扣（Leetcode）](https://leetcode.cn/problems/integer-break/description/)

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

**示例 1:**

```
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

**示例 2:**

```
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

**提示:**

- `2 <= n <= 58`

**题解思路**：

动规五步

- 确定`dp`数组及下标含义
  - `n`拆分为正整数的和的，这些整数的乘积的最大值
- 确定状态转移方程
  - `dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));`

- 初始化`dp`数组
  - `dp[0] = 0, dp[1] = 1, dp[2] = 1`

- 确定遍历顺序
  - 自底向上
- 手动推导`dp`数组...

**完整代码**

```c++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1, 0);
        dp[2] = 1;
        for(int i = 3; i <= n; i++){
            for(int j = 1; j <= i - 1; j++){
                dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));
            }
        }
        return dp[n];
    }
};
```

