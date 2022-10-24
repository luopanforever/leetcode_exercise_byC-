给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `*t*` 是否是 `*s*` 的字母异位词。

**注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `*s*` 和 `*t*` 互为字母异位词。

 

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

 

**提示:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` 和 `t` 仅包含小写字母



**解题思路：**

建立一个hash表，将字母a到字母z以数字索引的方式映射到hash表的索引，然后对题目所给字符串逐个检查并在对应位置添加或者减少个数。得到最终的hash表，如果hash表每个位置的值都还是0，则返回true，反之返回false



**代码：**

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int result[26] = {0}; //建立hash标  下标就对应的是a-z
        for(int i =0;i<s.size();i++){  //对第一个字符串映射hash表字符个数
            result[s[i] - 'a']++;
        }
        for(int i = 0;i<t.size();i++){  //对第二个字符串消除hash表字符个数
            result[t[i] - 'a']--;
        }
        for(int i =0;i<26;i++){
            if(result[i] != 0){
                return false;
            }
        }
        return true;
    }
};
```

