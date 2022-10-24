**题目描述**

给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

 

**示例：**

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
```

 

**提示：**

1. `1 <= S.length <= 20000`
2. `S` 仅由小写英文字母组成。

**题解思路:**

利用栈进行匹配，遍历string，如果s[i] == 栈.top() 则pop栈顶元素，否则push(s[i]),对于我来说关键难想的地方在于处理第一个元素的时候的判断条件，此时栈为空，如果访问top则会报错，可以采用此判断条件避免

`result.empty() || s[i] !=result.top()`如果第一条件成立则不会判断第二条件，故巧妙的避免了难点问题，一开始栈为空，故第一条件必定成立。

**代码：**

```c++
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> result;  
        result.push(s[0]);
        for(int i = 1; i < s.size(); i++){
            if(result.empty() || s[i] !=result.top()) result.push(s[i]);
            else result.pop();
        }
        string re;
        while(!result.empty()){
            re+=result.top();
            result.pop();
        }
        reverse(re.begin(),re.end());
        return re;
    }
};
```

