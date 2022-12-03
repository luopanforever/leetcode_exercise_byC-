## [647. 回文子串 - 力扣（Leetcode）](https://leetcode.cn/problems/palindromic-substrings/description/)

给你一个字符串 `s` ，请你统计并返回这个字符串中 **回文子串** 的数目。

**回文字符串** 是正着读和倒过来读一样的字符串。

**子字符串** 是字符串中的由连续字符组成的一个序列。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

**示例 1：**

```
输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例 2：**

```
输入：s = "aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 由小写英文字母组成

#### **题解思路：**

动归五步：

- `dp`数组以及下标含义
  - 布尔类型的`dp[i][j]`：表示区间范围`[i,j] `（注意是左闭右闭）的子串是否是回文子串，如果是`dp[i][j]`为`true`，否则为`false`。
- 确定递推公式
  - 两种情况
    - `s[i] == s[j]`
      - 查看`i`与`j`的距离
        - 如果是`<=1`则说明是回文（`a`, `aa`），`dp[i][j] = true`
        - 如果`>=2`则还需要看`[i + 1, j - 1]`的子序列是不是回文，如果是则`dp[i][j] = true`
    - `s[i] != s[j]`
      - 直接按照不是回文处理
- `dp`初始化
  - 一开始全部初始化为`false`，表示还没有开始匹配

- 遍历顺序 为保证`dp[i + 1][j - 1]`都是被用到过的  所以遍历要从下到上  从左到右

  由于只会遍历上三角，则j的索引从`i`开始

  ```c++
  for(int i = s.size() - 1; i >= 0; i--)
      for(int j = i; j < s.size(); j++)
  ```

#### **完整代码**

```c++
class Solution {
public:
    int countSubstrings(string s) {
    // dp数组的定义    
        // 布尔类型的dp[i][j]：表示区间范围[i,j] （注意是左闭右闭）的子串是否是回文子串，如果是dp[i][j]为true，否则为false。
        vector<vector<bool> > dp(s.size(), vector<bool>(s.size(), false));
    // 确定递推公式
        // s[i] == s[j]
            // i与j相等或者相差1  直接赋true
            // i与j相差2及以上  查看dp[i + 1][j - 1]是否为true
        // s[i] != s[j]
            // dp[i][j] = false;
    // 初始化
        int result = 0;
    // 遍历顺序 为保证dp[i + 1][j - 1]都是被用到过的  所以遍历要从下到上  从左到右
        for(int i = s.size() - 1; i >= 0; i--){
            for(int j = i; j < s.size(); j++){
                if(s[i] == s[j]){
                    if(j - i <= 1){
                        result++;
                        dp[i][j] = true;
                    }else if(dp[i + 1][j - 1]){
                        result++;
                        dp[i][j] = true;
                    }
                }
            }
        }
        
        return result;
    }
};
```

