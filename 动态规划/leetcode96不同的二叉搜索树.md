#### [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

### **题解思路：**

**动态规划：**

设`dp[i]`为从1到i互不相同的二叉树有多少种

**求状态转移方程：**

- `dp[1] = 1`
  - 就是左`dp[0]` * 右`dp[0]`
- `dp[2] = 2`
  - 左`dp[1] `* `右dp[0]`
  - 右`dp[0] `*` 左dp[1]`
- `dp[3] = 5`
  - 左`dp[2]` * `右dp[0]`
  - 左`dp[1] `* `右dp[1]`
  - 左`dp[0] `* `右dp[2]`

可见状态可由前面推出

由于没有节点的树也可以称为是一颗搜索二叉树，所以初始化`dp[0] = 1`

状态转移方程为

```c++
// 遍历每个头节点
for(int i = 1; i <= n; i++){
    // 累加每一种情况
    for(int j = 1; j <= i; j++){
        //dp[j - 1] 就是左dp  dp[i-j]就是右dp
        dp[i] += dp[j - 1] * dp[i - j];
    }
}
```

### **完整代码**

```c++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;
        // 以3为例子
        
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= i; j++){
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
```

