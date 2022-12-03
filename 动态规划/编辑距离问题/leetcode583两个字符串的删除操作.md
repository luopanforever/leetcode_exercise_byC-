## [583. 两个字符串的删除操作 - 力扣（Leetcode）](https://leetcode.cn/problems/delete-operation-for-two-strings/)

给定两个单词 `word1` 和 `word2` ，返回使得 `word1` 和 `word2` **相同**所需的**最小步数**。

**每步** 可以删除任意一个字符串中的一个字符。

**示例 1：**

```
输入: word1 = "sea", word2 = "eat"
输出: 2
解释: 第一步将 "sea" 变为 "ea" ，第二步将 "eat "变为 "ea"
```

**示例  2:**

```
输入：word1 = "leetcode", word2 = "etco"
输出：4
```

**提示：**

- `1 <= word1.length, word2.length <= 500`
- `word1` 和 `word2` 只包含小写英文字母

**题解思路：**

**`dp`数组的含义**:以`i-1`为结尾的字符串`word1`，和以`j-1`位结尾的字符串`word2`，想要达到相等，所需要删除元素的最少次数。

**递推公式：**

两种情况：

- 相等

  - 传递前一状态

    ```c++
    dp[i][j] = dp[i - 1][j - 1];
    ```

- 不相等

  - 情况一：删`word1[i - 1]`，最少操作次数为`dp[i - 1][j] + 1`
  - 情况二：删`word2[j - 1]`，最少操作次数为`dp[i][j - 1] + 1`
  - 情况三：同时删`word1[i - 1]`和`word2[j - 1]`，操作的最少次数为`dp[i - 1][j - 1] + 2`

  - 整合起来就是`dp[i][j] = min(dp[i - 1][j - 1] + 2, dp[i - 1][j] + 1, dp[i][j - 1] + 1);`

  ```c++
  min(dp[i - 1][j] + 1, dp[i][j - 1] + 1), dp[i - 1][j - 1] +2;
  
  // 由于dp[i - 1][j - 1] + 1 等于前两者其中一个 所以可以简化为
  min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
  ```

**`dp`数组的初始化**

分别初始化`dp[i][0]`和`dp[0][i]`，意思是删除到空串的步数

```python
for i in range(len(word1)):
    dp[i][0] = i;
for i in range(len(word2)):
    dp[0][i] = i;
```

**遍历顺序**

从前到后

**完整代码：**

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int> > dp(word1.size() + 1, vector<int>(word2.size()+1));

        for(int i = 0; i <= word1.size(); i++) dp[i][0] = i;
        for(int i = 0; i <= word2.size(); i++) dp[0][i] = i;
        for(int i = 1; i <= word1.size(); i++){
            for(int j = 1; j <= word2.size(); j++){
                if(word1[i - 1] == word2[j - 1]) 
                    dp[i][j] = dp[i - 1][j - 1];
                else 
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```

