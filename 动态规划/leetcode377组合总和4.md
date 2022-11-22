## [377. 组合总和 Ⅳ - 力扣（Leetcode）](https://leetcode.cn/problems/combination-sum-iv/description/)

给你一个由 **不同** 整数组成的数组 `nums` ，和一个目标整数 `target` 。请你从 `nums` 中找出并返回总和为 `target` 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

 

**示例 1：**

```
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

**示例 2：**

```
输入：nums = [9], target = 3
输出：0
```

 

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- `nums` 中的所有元素 **互不相同**
- `1 <= target <= 1000`



#### **题解思路：**

**关键字**：动归、完全背包

- 物品数量  `nums.size()`
- 背包大小 `target`
- 物品价值 `nums[i]`
- 物品重量 `nums[i]`

本题题解`[2,1]、[1,2]`是不同解，所以遍历顺序要改变，先遍历背包，再遍历物品

```c++
for(int j = 0; j <= target; j++){
    for(int i = 0; i < nums.size(); i++){
       	if(j >= nums[i] && dp[j] < INT_MAX - dp[j - nums[i]])  // 防止溢出
            dp[j] += dp[j - nums[i]];
    }
}
```

**`dp`数组含义**

`dp[j]`：装满`j`大小的背包有多少种方法

**初始化**

由于是压缩一维`dp`且是求装入背包的总方法

所以初始化`dp[0] = 1`

**递推公式**

求装入背包方法基本都是如下形式

` dp[j] += dp[j - nums[i]];`

#### 完整代码：

```c++
class Solution {
public:     
    int combinationSum4(vector<int>& nums, int target) {
        vector<long long> dp(target + 1, 0);
        dp[0] = 1;

        for(int j = 0; j <= target; j++){
            for(int i = 0; i < nums.size(); i++){
                if(j >= nums[i] && dp[j] < INT_MAX - dp[j - nums[i]]) 
                    dp[j] += dp[j - nums[i]];
            }
        }
        return dp[target];
    }
};
```

