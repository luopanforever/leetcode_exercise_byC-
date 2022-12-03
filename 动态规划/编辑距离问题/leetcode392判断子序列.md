## [392. 判断子序列 - 力扣（Leetcode）](https://leetcode.cn/problems/is-subsequence/description/)

给定字符串` s `和` t `，判断` s `是否为` t `的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**进阶：**

如果有大量输入的` S`，称作` S1, S2, ... , Sk `其中` k >= 10`亿，你需要依次检查它们是否为 `T `的子序列。在这种情况下，你会怎样改变代码？

**致谢：**

特别感谢 [@pbrother ](https://leetcode.com/pbrother/)添加此问题并且创建所有测试用例。

**示例 1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false
```

**提示：**

- `0 <= s.length <= 100`
- `0 <= t.length <= 10^4`
- 两个字符串都只由小写字符组成。

#### **题解思路：**

**双指针**，轻松解决

```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int sp = 0;
        int tp = 0;
        while(sp < s.size() && tp < t.size()){
            if(s[sp] == t[tp]){
                sp++;
                tp++;
                continue;
            }
            tp++;
        }   
        if(sp == s.size()) return true;
        return false;
    }
};
```

**动态规划**

与以往的找非连续递增子序列不同，本题不是数字，不可以比较大小。

要求非连续，但是相对顺序要一样，所以如果遇见了不相等的则保留上一状态

因此递推公式为：

```c++
if(s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
else dp[i][j] = dp[i][j - 1];
```

最后判断`dp[i][j]`的大小是否等于子串`s`的长度即可

**完整代码**

```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        // dp[i][j]以i - 1和j - 1为结尾的子序列的最大长度
        vector<vector<int> > dp(s.size() + 1, vector<int>(t.size() + 1, 0));
        int result = 0;

        for(int i = 1; i <= s.size(); i++){
            for(int j = 1; j <= t.size(); j++){
                if(s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = dp[i][j - 1];
            }
            
        }
        return dp[s.size()][t.size()] == s.size();
    }
};   
```

