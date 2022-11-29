## [718. 最长重复子数组 - 力扣（Leetcode）](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/description/)

给两个整数数组 `nums1` 和 `nums2` ，返回 *两个数组中 **公共的** 、长度最长的子数组的长度* 。

**示例 1：**

```
输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
输出：3
解释：长度最长的公共子数组是 [3,2,1] 。
```

**示例 2：**

```
输入：nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
输出：5
```

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 100`

#### **题解思路：**

`dp`数组含义：`dp[i][j]`表示以以`i-1`结尾的`nums1`和以`j-1`结尾的`nums2`的重复子数组的最大长度

递推公式：

如果`nums1[i - 1] == nums2[j - 1]`则`dp[i][j] = dp[i - 1][j - 1] + 1`;

```c++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        // dp[i][j] -> 以i-1结尾的nums1和以j-1结尾的nums2的最大重复子数组的长度
        vector<vector<int> > dp(nums1.size() + 1, vector<int>(nums2.size() + 1, 0));

        int result = 0;

        for(int i = 1; i <= nums1.size(); i++){
            for(int j = 1; j <= nums2.size(); j++){
                if(nums1[i - 1] == nums2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1; 
                result = result > dp[i][j] ? result : dp[i][j];
            }
            
        }
        return result;
    }
};
```

