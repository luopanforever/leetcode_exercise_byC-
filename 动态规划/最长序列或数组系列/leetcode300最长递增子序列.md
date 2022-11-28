## [300. 最长递增子序列 - 力扣（Leetcode）](https://leetcode.cn/problems/longest-increasing-subsequence/description/)

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

**进阶：**

- 你能将算法的时间复杂度降低到 `O(n log(n))` 吗?

#### **题解思路:**

**动态规划、**

核心思路点： 对于索引`i`所在的数字，如果其前面有小于他的数则子序列长度加`1`，这就是递推公式的由来，动态规划是从0索引开始，所以`dp`一定是最大且一直递增

- **`dp`数组的含义**  
  - 以索引`i`结尾的最长子序列**长度**

- 确定递推公式

  - 位置`i`的最长升序子序列等于`j`从`0`到`i-1`各个位置的最长升序子序列` + 1` 的最大值。

    `if(nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);` 

- 初始化
  - 最小都是`1`，所以全部初始化为`1`

- 遍历顺序，根递推公式  从前到后

#### **完整代码**

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        // 每个物品价值1
        if(nums.size() <= 1) return nums.size();
        // dp[i] 代表以索引i字符结尾的最长递增子序列
        vector<int> dp(nums.size(), 1);
        // 初始化 最短都是1
        
        int result = 0;
        for(int i = 1; i < nums.size(); i++){
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1); 
            }
            if(dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```

