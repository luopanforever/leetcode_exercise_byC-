## [1143. 最长公共子序列 - 力扣（Leetcode）](https://leetcode.cn/problems/longest-common-subsequence/description/)

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

**示例 1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**示例 2：**

```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

**示例 3：**

```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。
```

**提示：**

- `1 <= text1.length, text2.length <= 1000`
- `text1` 和 `text2` 仅由小写英文字符组成。

#### **题解思路**

本题和[动态规划：718. 最长重复子数组 (opens new window)](https://programmercarl.com/0718.最长重复子数组.html)区别在于这里不要求是连续的了，但要有相对顺序

**`dp`数组的定义**

- 与`718`题一样，`dp[i][j]`都是代表以`i-1`和`j-1`结尾的两个字符串的最大公共子序列长度

**确定递推公式**

- 分两种情况
  - `text[i - 1] == text[j - 1]`：
    - `dp[i][j] = dp[i - 1][j - 1] + 1;`
  - `text[i - 1] != text[j - 1]`
    -  **如果`text1[i - 1]` 与` text2[j - 1]`不相同，那就看看`text1[0, i - 2]`与`text2[0, j - 1]`的最长公共子序列 和` text1[0, i - 1]`与`text2[0, j - 2]`的最长公共子序列，取最大的。**
    - `dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);`

**完整代码：**

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int> > dp(text1.size() + 1, vector<int>(text2.size() + 1, 0));
        int result = 0;
        for(int i = 1; i <= text1.size(); i++){
            for(int j = 1; j <= text2.size(); j++){
                if(text1[i - 1] == text2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                result = result >dp[i][j] ? result : dp[i][j];
            }
        }
        return result;
    }
};
```

