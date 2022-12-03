## [72. 编辑距离 - 力扣（Leetcode）](https://leetcode.cn/problems/edit-distance/)

给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

**提示：**

- `0 <= word1.length, word2.length <= 500`
- `word1` 和 `word2` 由小写英文字母组成

#### **题解思路：**

**动态规划**

- 确定`dp`数组及下标含义
  - `dp[i][j]`：以下标为i - 1为结尾的子序列转化整以j - 1为结尾的子序列所需的最少操作数

- 确定递推公式

  - 相等

    - 传递前一状态

      ```c++
      dp[i][j] = dp[i - 1][j - 1];
      ```

  - 不相等

    - **`word2`添加一个元素，相当于`word1`删除一个元素**，所以下述操作都是以`word1`的角度来看的

    - 增加

      - 在`word1`的`i - 2`后进行添加操作

        `dp[i][j] = dp[i - 1][j] + 1 ;`

    - 删除

      - 在`word2`的`j - 2 `后进行添加操作

        `dp[i][j] = dp[i][j - 1] + 1;`

    - 替换

      - 替换元素，`word1`替换`word1[i - 1]`，使其与`word2[j - 1]`相同，此时不用增加元素，那么以下标`i-2`为结尾的`word1` 与 `j-2`为结尾的`word2`的最近编辑距离 加上一个替换元素的操作。

        `dp[i][j] = dp[i - 1][j - 1] + 1`

**`dp`数组的初始化**

`dp[i][0]`初始化为`i`代表执行`i`个删除操作得到空串

`dp[0][i]`初始化为`i`代表执行`i`个添加操作得到空串

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        // dp[i][j] 以i - 1为结尾的子序列转化整以j - 1为结尾的子序列所需的最少操作数
        vector<vector<int> > dp(word1.size() + 1, vector<int>(word2.size() + 1, 0));

        for(int i = 0; i <= word1.size(); i++) dp[i][0] = i;
        for(int i = 0; i <= word2.size(); i++) dp[0][i] = i;

        for(int i = 1; i <= word1.size(); i++){
            for(int j = 1; j <= word2.size(); j++){
                if(word1[i - 1] == word2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1];
                else 
                    dp[i][j] = min(min(dp[i - 1][j] + 1, dp[i][j - 1] + 1), dp[i - 1][j - 1] + 1);
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```

