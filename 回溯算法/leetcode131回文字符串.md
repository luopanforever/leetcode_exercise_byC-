#### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

**示例 1：**

```c++
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```c++
输入：s = "a"
输出：[["a"]]
```

**提示：**

- `1 <= s.length <= 16`
- `s `仅由小写英文字母组成

#### **题解思路**

定义两个全局变量：

- `vector<vector<string> > result;`
  - 保存结果总和
- `vector<string> tmp;`
  - 用来存储每一种结果

c++要用到的函数：

```c++
basic_string substr( size_type index, size_type num = npos);
// index 是从哪个位置开始截取
// num 是截取多少个字符
```

利用**回溯算法**来穷举

**给出回溯模板:**

```c++
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (//选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        //处理节点;
        backtracking(路径，选择列表); // 递归
       // 回溯，撤销处理结果
    }
}
```

回溯也是递归，所以也可以按照递归三步走

- 确定参数和返回值

  - 回溯算法不需要返回值

  - 本题涉及截取子串，所以要增加一个参数，用来表示开始截取的位置

    ```c++
    void backtracking(string s, int start);
    ```

- 确定终止条件

  - 当截取的开始位置大于等于了字符串长度，则将其加入到`结果二维数组`

    ```c++
    if(start >= s.size()){
        result.push_back(tmp);
        return ;
    }
    ```

- 确定本层逻辑

  - 本层逻辑其实就是确定`for`循环里面的内容

  - 假设输入样例是`"aab"`，`backtracking`的初始参数为`(s, 0)`

  - 直接给出核心代码 我解释他们的意思

    ```c++
    for(int i = start; i < s.size(); i++){
        if(isHuiwen(start, i, s)){
            tmp.push_back(s.substr(start, (i - start) + 1));
        }else {
            continue;
        }
        backtracking(s, i + 1);
        tmp.pop_back();
    }
    ```

    当`i`等于0的时候的那一层`for`循环 -> 一定会出一个结果，就是每个字符单独的时候也就是`["a", "a", "b"]`，当`i++`后，`i`变成`1`，`substr`里面的函数就会是`(0, 1)`就会截取`"aa"`字符，之后再添加一个`"b"`，形成第二种结果，在最后一次外层`for`循环时，`i`为`2`，此时截取的字符为`"aab"`，则直接结束。   。。。我已经看不下去我解释的了 以后再来该改改吧

    - 反正要想得通的一点是，对于最外层的`for`循环（没有进入递归的时候），`start`一直是`0`，进入后因为传参是加了`1`所以`start`才会变

**完整代码**

```c++
class Solution {
public:
    bool isHuiwen(int left, int right, const string &a){
        for(int i = left, j = right; i < j; i++, j--){
            if(a[i] != a[j]) return false;
        }
        return true;
    }
    vector<vector<string>> result;
    vector<string> tmp;
    void backtracking(string s, int start){
        if(start >= s.size()){
            result.push_back(tmp);
            return ;
        }
        for(int i = start; i < s.size(); i++){
            if(isHuiwen(start, i, s)){
                tmp.push_back(s.substr(start, (i - start) + 1));
            }else {
                continue;
            }
            backtracking(s, i + 1);
            tmp.pop_back();
        }
    }
    vector<vector<string>> partition(string s) {
        backtracking(s, 0);
        return result;
    }

};
```

