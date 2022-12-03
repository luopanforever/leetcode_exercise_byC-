## [115. 不同的子序列 - 力扣（Leetcode）](https://leetcode.cn/problems/distinct-subsequences/description/)

给定一个字符串 `s` 和一个字符串 `t` ，计算在 `s` 的子序列中 `t` 出现的个数。

字符串的一个 **子序列** 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，`"ACE"` 是 `"ABCDE"` 的一个子序列，而 `"AEC"` 不是）

题目数据保证答案符合 32 位带符号整数范围。

**示例 1：**

```
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

**示例 2：**

```
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下图所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag
```

**提示：**

- `0 <= s.length, t.length <= 1000`
- `s` 和 `t` 由英文字母组成

**题解思路：**

**`dp`数组的含义：**

- `dp[i][j]`以`i-1`为结尾的`s`子序列中出现以`j-1`为结尾的`t`的个数

**`dp`数组的初始化**

- `dp[i][0] = 1`表示以`i-1`位结尾的子序列中出现空串的个数为`1`
- `dp[0][i] = 0 `表示空串中出现以`i - 1`结尾的子序列的个数为`0` 

**递推公式：**

- 分两种情况：
  - `s[i - 1] `与` t[j - 1]`相等
    - `dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];`
    - 由两部分组成，一部分是由s[i - 1]推导另一部分不由s[i - 1]推出
    - 例如`s：bagg` 和 `t：bag` ，`s[3] `和` t[2]`是相同的，但是字符串`s`也可以不用`s[3]`来匹配，即用`s[0]s[1]s[2]`组成的`bag`。
  - `s[i - 1] `与` t[j - 1] `不相等
    - 只能不用`s[i - 1]`来匹配
    - `dp[i][j] = dp[i - 1][j]`

**完整代码:**

```c++
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<unsigned long> > dp(s.size() + 1, vector<unsigned long>(t.size() + 1, 0));
        for(int i = 0; i < s.size(); i++){
            dp[i][0] = 1;
        }
        for(int i = 1; i <= s.size(); i++){
            for(int j = 1; j <= t.size(); j++){
                if(s[i - 1] == t[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                else 
                    dp[i][j] = dp[i - 1][j];
            }
        }
    
        return dp[s.size()][t.size()];
    }
};
```

