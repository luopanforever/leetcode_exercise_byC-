#### 题目描述

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

 

**示例 1：**

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

 

**限制：**

```
0 <= s 的长度 <= 10000
```



**题解思路:**

一开始不加思考，额外开辟了一个空间，秒通过了这道题

```c++
class Solution {
public:
    string replaceSpace(string s) {
        string result;
        for(int i =0;i < s.size();i++){
            if(s[i] == ' '){
                result+="%20";
                continue;
            }
            result+=s[i];
        }
        return result;
    }
};
```

但是剑指offer出这道题肯定不是想让我这样过，但是我确实不知道如何原地修改，于是看了carl的题解，知道可以利用双指针的办法来解决。

先计算有多少个空格，然后重新为该字符串分配大小，然后一个新指针和一个旧指针，从后向前进行操作。

**此类问题可以看做是数字填充类的问题，可以先预先给数组扩容带填充后的大小，然后在从后向前进行操作。**

旧指针指向原字符串尾，新指针指向新字符串尾。从后向前操作，当旧指针指向非空格字符串的时候，将旧指针的值赋给新指针，否则将新指针以及新指针的前两个位置赋值为target值（%20），结束条件是旧指针>=新指针



**代码：**

```c++
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0;
        for(int i =0;i<s.size();i++){
            if(s[i] == ' ') count++;
        }
        int oldsize = s.size();
        s.resize(s.size() + 2*count);
        int newsize = s.size();
        for(;oldsize < newsize;newsize--,oldsize--){
            if(s[oldsize] == ' '){
                s[newsize--] = '0';
                s[newsize--] = '2';
                s[newsize] = '%';
            }else{
                s[newsize] = s[oldsize];
            }
        }
        return s;
       }
};
```

