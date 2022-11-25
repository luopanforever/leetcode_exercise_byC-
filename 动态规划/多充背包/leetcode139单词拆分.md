## [139. 单词拆分 - 力扣（Leetcode）](https://leetcode.cn/problems/word-break/)

给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。请你判断是否可以利用字典中出现的单词拼接出 `s` 。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

 

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

 

**提示：**

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` 和 `wordDict[i]` 仅有小写英文字母组成
- `wordDict` 中的所有字符串 **互不相同**



### **题解思路：**

在`wordDict`中凑单词，是否能凑成`s`字符串，可以取重复单词，则想到多重背包。

返回的是`bool`，则`dp`数组存储`bool`，那么此题`dp`数组的含义是？

#### **`dp`数组含义**

- `dp[j]` ： `dp[j]` 字符串长度为`j`的话，`dp[j]`为`true`时则表示可以拆分为一个或多个在字典中出现的单词

#### **遍历顺序：**

本题最终要求的是是否都出现过，所以对出现单词集合里的元素是组合还是排列，并不在意

- 如果求组合数就是外层`for`循环遍历物品，内层`for`遍历背包。
- 如果求排列数就是外层`for`遍历背包，内层`for`循环遍历物品。

本题上述两个遍历方式都是完全一样的，我选择先遍历背包再遍历物品。

**还要确定四项：**

- 物品数量：`s.size()`
- 背包大小：`s.size()`
- 物品重量：`s.substr(i, j - i);`
- 物品价值：无法描述

#### `dp`数组初始化

`dp[0]`为`true`，代表着空字符串可以被拆分成`wordDict`中的字串，相当于一个空串吧

其他的初始化为`false`，因为都是不确定的，需要后续推导

#### **递推公式**：

**核心!!!!!!!!!!!!超重要  本题关键！ 与其他多重背包的核心区别点**

时刻记住`dp`数组的含义

**`dp[j]`的意思**是长度为`[j]`的字符串**是否**可以被拆分成`wordDict`中的字符串

在`s`字符串中，从开头数 长度为`i`的字符串能被拆分成`wordDict`中的字符串，且从长度i开始到长度`j`这一段字串也能在`wordDict`中找到，则说明`dp[j]`为`true`

转换为下列等价代码

```c++
if(dp[i] && wordSet.find(s.substr(i, j - i)) != wordSet.end()) 
    dp[j] = true;
```

**完整代码**

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());

        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;

        for(int j = 1; j <= s.size(); j++){  // 遍历背包  
            for(int i = 0; i < s.size(); i++){  // 遍历物品
                if(i < j && dp[i] && wordSet.find(s.substr(i, j - i)) != wordSet.end()) dp[j] = true;
            }
        }
        return dp[s.size()];
    }
};
```

- 为什么遍历背包是`<=`而遍历物品是`<`
  - 背包那个位置代表`bagweigt`
    - `dp`数组初始化大小就是`s.size() + 1`
  - 物品那个位置代表`weight.size()`
    - 物品数量只看数量 从索引 开始