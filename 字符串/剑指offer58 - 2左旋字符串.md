#### 题目描述

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

 

**示例 1：**

```
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```

**示例 2：**

```
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```

 

**限制：**

- `1 <= k < s.length <= 10000`

#### 题解思路

**思路一**

创建额外空间保存左边的字符串，然后对原字符串左移，再把额外空间保存的字符串替换在后边即可

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        int fast = 0;
        int slow = s.size() - n;
        string temp(s.begin(),s.begin()+n);
        for(int i =0;i<s.size()-n;i++){
            s[i]=s[i+n];
        }
        for(int i =slow ;i<s.size();i++){
            s[i] = temp[fast++]; 
        }
        return s;
    }
};

```



**思路二：**

不申请任何空间的前提下解题，思路为首先整体翻转，然后根据k分段翻转

```c++
class Solution {
public:
    void reverseWords(string &s,int start,int end){
        int left = start;
        int right = end - 1;
        while(left<right){
            swap(s[left++],s[right--]);
        }
    }
    string reverseLeftWords(string s, int n) {
        //思路 先整体翻转  再局部翻转
        
        //翻转整个字符串
        reverseWords(s,0,s.size());
        //以n为截断处，分别翻转前后两部分
        //翻转前面的部分
        reverseWords(s,0,s.size()-n);
        //翻转后面的部分
        reverseWords(s,s.size() - n,s.size());
        return s;
    }
};
```

