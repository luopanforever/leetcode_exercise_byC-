## [53. 最大子数组和 - 力扣（Leetcode）](https://leetcode.cn/problems/maximum-subarray/description/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。

#### **题解思路:**

**动归五步：**

- 确定`dp`数组及下标含义
  - `dp[i] `：代表`[0, i]`的最大子序列和

- 确定递推公式

  - **考虑一直累加，动态的初始化起始位置**

    ```c++
    dp[i] = max(dp[i - 1] + nums[i], nums[i]);
    ```

- 初始化

  - `dp[0] = nums[0]`
  - 这是能够确定的

- 确定遍历顺序

  - 根据递推公式`i`由`i - 1`推出，则从前到后

- 手动推导。。。。

#### **完整代码**

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // dp[i] 代表 [0, i] 的最大连续和
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        int result = dp[0];
        for(int i = 1; i < nums.size(); i++){
            dp[i] = max(dp[i - 1] + nums[i], nums[i]);
            if(result < dp[i]) result = dp[i];
        }
        return result;
    }
};
```

