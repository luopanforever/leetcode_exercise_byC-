**题目描述**

根据[ 逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437)，求表达式的值。

有效的算符包括 `+`、`-`、`*`、`/` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

**注意** 两个整数之间的除法只保留整数部分。

可以保证给定的逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

 

**示例 1：**

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

**示例 2：**

```
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

**示例 3：**

```
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 

**提示：**

- `1 <= tokens.length <= 104`
- `tokens[i]` 是一个算符（`"+"`、`"-"`、`"*"` 或 `"/"`），或是在范围 `[-200, 200]` 内的一个整数



**题解思路：**

可以利用栈来逆波兰表达式求值，当遇到字符串，则对字符串前面的两个元素进行操作然后重新push进栈，当遇到数字就进栈，最后栈里就会只剩下一个值，那就是最终表达式的值

**代码：**

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> result;
        for(string s:tokens){
            if(s == "+"||s=="-"||s=="*"||s=="/"){
                int second = result.top();
                result.pop();
                int first = result.top();
                result.pop();
                if(s=="+"){
                    int re = first + second;
                    result.push(re);
                }else if(s=="-"){
                    int re = first - second;
                    result.push(re);
                }else if(s=="*"){
                    int re = first * second;
                    result.push(re);
                }else if(s=="/"){
                    int re = first / second;
                    result.push(re);
                }
            }else{
                result.push(stoi(s));
            }
        }
        return result.top();
    }
};
```

