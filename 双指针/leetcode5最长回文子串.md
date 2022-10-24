#### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

**题解思路**

- 中心向两端的双指针

**完整代码：**

// `right`变量可以不要

```c++
class Solution {
public:
    int left = 0;
    int right = 0;
    int maxLength = 0;
    /*
    s.substr(int index, int num); // 返回从s的index 开始的num个字符
    */
    string longestPalindrome(string s) {
        for(int i = 0; i < s.size(); i++){
            HuiWenExtend(s, i, i);
            HuiWenExtend(s, i, i + 1);
        }
        return s.substr(left, maxLength);
    }
    void HuiWenExtend(string s, int i, int j){
        while(i >= 0 && j < s.size() && s[i] == s[j]){
            if(j - i + 1 > maxLength){
                left = i;
                right = j;
                maxLength = j - i + 1;
            }
            i--;
            j++;
        }
    }
};
```

