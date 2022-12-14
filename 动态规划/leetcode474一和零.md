### [474. 一和零 - 力扣（Leetcode）](https://leetcode.cn/problems/ones-and-zeroes/)

给你一个二进制字符串数组 `strs` 和两个整数 `m` 和 `n` 。

请你找出并返回 `strs` 的最大子集的长度，该子集中 **最多** 有 `m` 个 `0` 和 `n` 个 `1` 。

如果 `x` 的所有元素也是 `y` 的元素，集合 `x` 是集合 `y` 的 **子集** 。

 

**示例 1：**

```
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```

**示例 2：**

```
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
```

 

**提示：**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` 仅由 `'0'` 和 `'1'` 组成
- `1 <= m, n <= 100`



**题解思路**：

细解问题：请你找出并返回`strs`的最大子集的长度，该子集中最多有`m`个`0`和`n`个`1`

则`dp`数组的含义一目了然，是一维`dp`，但是背包有两个维度，一个维度存储`0`的个数一个维度存储`1`的个数，`dp[i][j]`的含义就是最多有`i`个`0`和`j`个`1`的`strs`的最大子集的大小

**连环四问：**

- 有几个物品供选择
  - `strs.size()`个
- 背包最大容量
  - `[m,n]` 行存0  列存1 
- 单个物品重量
  - `1`的个数与`0`的个数

- 单个物品的价值
  - 为`1`，`dp`数组求最大子集的长度，每个单位都是`1`

**完整代码：**

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) { 
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for(int i = 0; i < strs.size(); i++){
            int oneN = 0;
            int zeroN = 0;
            for(auto c: strs[i]){
                if(c != '0') oneN++;
                else zeroN++;
            }
            for(int j = m; j >= zeroN; j--){
                for(int k = n; k >= oneN; k--){
                    dp[j][k] = max(dp[j][k], dp[j - zeroN][k - oneN] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

