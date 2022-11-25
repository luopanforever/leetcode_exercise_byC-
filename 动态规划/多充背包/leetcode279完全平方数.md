## [279. 完全平方数 - 力扣（Leetcode）](https://leetcode.cn/problems/perfect-squares/description/)

给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

**提示：**

- `1 <= n <= 104`

#### 题解思路

转化为多重背包，从一些完全平方数中选择，凑成n值，对应从一堆数中选择，且相同的数字可以重复选择，`dp`数组意义则是装满该背包的最小数量

- 根据题意 构造一个完全平方和数组

  ```c++
  int boundary = sqrt(n);
  vector<int> nums(boundary + 1, 0);
  for(int i = 0; i < nums.size(); i++){
      nums[i] = i * i;
  }
  ```

确定四样东西：

- 物品数量` nums.size();`
- 背包大小` n`
- 物品重量 `nums[i]`
- 物品价值` 1`

**动归五步：**

- `dp[j]`数组含义：装满大小为`j`的背包的最小数量

  ` vector<int> dp(n + 1, INT_MAX);`

- `dp`数组的初始化:

  `dp[0] = 0 ` 其他的设置为`INT_MAX`

- 递推公式：

  - 求数量基本都是如下

    ```c++
    dp[j] += dp[j - nums[i]];
    ```

- 遍历顺序

  - `[1, 2]、 [2, 1]`是一个解集，所以先遍历物品再遍历背包

    ```c++
    for(int i = 0; i < nums.size(); i++){
        for(int j = nums[i]; j <= n; j++){
            // 防止溢出 因为初始化为INTMAX后+1会溢出 如果是求最大的话则不需要
            if(dp[j - nums[i]] != INT_MAX)
                dp[j] = min(dp[j], dp[j - nums[i]] + 1);
        }
    }
    ```

#### 完整代码

```c++
class Solution {
public:
    int numSquares(int n) {
        int boundary = sqrt(n);
        vector<int> nums(boundary + 1, 0);
        for(int i = 0; i < nums.size(); i++){
            nums[i] = i * i;
        }
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for(int i = 0; i < nums.size(); i++){
            for(int j = nums[i]; j <= n; j++){
                if(dp[j - nums[i]] != INT_MAX)
                    dp[j] = min(dp[j], dp[j - nums[i]] + 1);
            }
        }
        return dp[n];
    }
};
```

上述代码在开始构造一个`nums`数组是为了理解，多了点空间复杂度，下列代码就是优化过的

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for(int i = 0; i <= sqrt(n); i++){
            for(int j = i * i; j <= n; j++){
                if(dp[j - i*i] != INT_MAX)
                    dp[j] = min(dp[j], dp[j - i*i] + 1);
            }
        }
        return dp[n];
    }
};
```

