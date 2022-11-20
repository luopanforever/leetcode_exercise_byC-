### [494. 目标和 - 力扣（Leetcode）](https://leetcode.cn/problems/target-sum/description/)

给你一个整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

**示例 1：**

```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**示例 2：**

```
输入：nums = [1], target = 1
输出：1
```

**提示：**

- `1 <= nums.length <= 20`
- `0 <= nums[i] <= 1000`
- `0 <= sum(nums[i]) <= 1000`
- `-1000 <= target <= 1000`

未解决二维dp的组合方式

**完整代码**

思路都在里面了

```c++
class Solution {
public:
    // 本题使用动态规划也就是分成两堆，正堆和负堆  这样就有一个等式  正堆 - (sum - 正堆) = target，就是求能够凑成正堆的有多少种， dp[i][j]的意义就是从0-i中选能够凑成j容量的方法有多少种
    int findTargetSumWays(vector<int>& nums, int target) {
        // 首先是数学推导  sum如果小于target则返回0中  (sum+target)%2 == 1则返回0 因为考虑了向下取整的影响
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if(sum < target) return 0;
        if((sum + target)%2 == 1) return 0;
        if(abs(target) > sum) return 0;
        int bagWeight = (sum + target) / 2;
        // 背包容量是多少  背包中有多少个物品  每个物品的重量和价格？
        // 二维dp  意义？ -> 从[i, nums.size()]中选一个物品到容量为j的背包，所获得的的最大价值是？
        // 这里的最大价值的意思是最多有多少种方法
        /*
        vector<vector<int> > dp(nums.size(), vector<int>(bagWeight + 1, 0));

        // 初始化？  不知道怎么办。。。  就按照常规的。。。
        // dp[i][0]第0列初始化为1，根据意义，放入背包0的最大方法就是不放  为1中
        for(int i = 0; i < nums.size(); i++){
            dp[i][0] = 1;
        }
        // dp[0][j] 第0索引东西装满j容量背包的最大方法，则只需初始化dp[0][0] 和dp[0][nums[0]]，因为只有不装和装满两个选择
        dp[0][nums[0]] = 1;

        // 动态规划  状态转移方程 -> dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - ...不会])  
        // 求得是个数 所以一般的推导形式是以下形式dp[i][j] += dp[i - 1][j - nums[i]];
        for(int i = 1; i < nums.size(); i++){
            for(int j = 1; j < bagWeight; j++){
                if(j < nums[i]) dp[i][j] += dp[i - 1][j];
                else dp[i][j] += dp[i - 1][j - nums[i]];
            }
        }
        return dp[nums.size() - 1][bagWeight];
        */
        // 一维dp
        // dp数组含义  背包容量为j下的不同方法
        vector<int> dp(bagWeight + 1, 0);
        // 初始化 初始化dp[0] = 1意思是装满背包容量为0的背包一共有1种方法，那就是不装。。。
        dp[0] = 1;
        // 动态规划
        for(int i = 0; i < nums.size(); i++ ){
            for(int j = bagWeight; j >= nums[i]; j--)
                dp[j] += dp[j - nums[i]];
        }
        for(auto i: dp){
            cout<<i<<" ";
        }
        
        return dp[bagWeight];
    }
};
```

