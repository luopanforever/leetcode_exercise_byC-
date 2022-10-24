给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

 

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**示例 4：**

```
输入：s = "([)]"
输出：false
```

**示例 5：**

```
输入：s = "{[]}"
输出：true
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

**题解思路：**

利用栈进行匹配，遍历字符串，遇到左括号就push对应的右括号，于是会产生三种情况

- 左边括号多余
  - 如果遍历完字符串的话发现stack中还有元素则返回false
- 括号不多余但是发生了不匹配的现象
  - 栈顶元素与当前string遍历位置的char不符合
- 右边括号多余
  - 在遍历string的过程中，stack提前为空

**代码：**

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> temp;
        for(int i =0;i<s.size();i++){
            if(s[i] == '(') temp.push(')');  //添加对应的右括号，便于后续判断
            else if(s[i] == '{') temp.push('}');
            else if(s[i] == '[') temp.push(']');
            else if(temp.empty() || temp.top() != s[i]) return false;  //第二种和第三种情况，
            else temp.pop();  //匹配成功
        }
        return temp.empty();  //stack还有剩余则为第一种情况，返回false
    }
};
```

