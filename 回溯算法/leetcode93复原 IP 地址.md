#### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

**提示：**

- `1 <= s.length <= 20`
- `s` 仅由数字组成

#### **题解思路**

要用到的方法：

```c++
// string类的insert方法和erase方法

// 在迭代器所指位置前插入一个字符
s.insert(iterator it, char a);
// 在迭代器所指位置删除一个字符
s.erase(iterator it);

// 单个字符转换为数字
char a_char  = '1';
int a_int = a_char - '0';
```

**思路：**

本题是给定一个字符串，将其还原为IP地址，可以将其划为分割问题，分割问题就可以用回溯算法来穷举所有目标项

回溯就是递归，可以按照递归三步走

- 确定函数的返回值和参数列表

  - 还原ip地址需要在字符串中加`.`这一元素，可以提供一个参数来统计点的个数
  - 这题肯定是不能重复元素分割，所以还要有一个`startIndex`来标记切割的起始位置。	
  - 回溯无需返回值

- 确定终止条件

  - 终止条件就可以利用`pointerNumber`，只有当`pointerNumber`达到了3才会进入终止条件，然后就顺其自然的查看最后一个点后的数是否合格来决定是否加入结果目录

    ```c++
    if(pointNumber == 3){
        if(isValid(s, startIndex, s.size() - 1))
            result.push_back(s);
        return ;
    }
    ```

- 确定本层逻辑

  - 本层逻辑也就是回溯逻辑，根据模板，其通常都是一个`for`循环里面套一个递归，整个回溯可以抽象成一个树形结构，`for`循环遍历宽度，递归遍历深度。其他解释不了勒，成为高高手了在来描述，现在很难描述出来，

    ```c++
    for(int i = startIndex; i < s.size(); i++){
        if(isValid(s, startIndex, i)){
            s.insert(s.begin() + i + 1, '.');
            pointNumber++;
            backtracking(s, i + 2, pointNumber);
            pointNumber--;
            s.erase(s.begin() + i + 1);
        }else break;
    }
    ```

`valid`**函数**

- 判断第四段子字符串是否合法，如果合法就放进`result`中
- 0开头的数字不合法
- 遇到非数字字符不合法
- 如果大于255了不合法

```c++
bool isValid(string s, int start, int end){
    if(start > end) return false;

    if(s[start] == '0' && start != end) return false;

    int sum = 0;
    for(int i = start; i <= end; i++){
        if(s[i] < '0' || s[i] > '9') return false;

        sum = sum * 10 + (s[i] - '0');
        if(sum > 255) return false;
    }
    return true;
}
```

**完整代码**

```c++
class Solution {
public:
    vector<string> result;
    
    bool isValid(string s, int start, int end){
        if(start > end) return false;

        if(s[start] == '0' && start != end) return false;

        int sum = 0;
        for(int i = start; i <= end; i++){
            if(s[i] < '0' || s[i] > '9') return false;

            sum = sum * 10 + (s[i] - '0');
            if(sum > 255) return false;
        }
        return true;
    }
    void backtracking(string s, int startIndex, int pointNumber){
        if(pointNumber == 3){
            if(isValid(s, startIndex, s.size() - 1))
                result.push_back(s);
            return ;
        }
        for(int i = startIndex; i < s.size(); i++){
            if(isValid(s, startIndex, i)){
                s.insert(s.begin() + i + 1, '.');
                pointNumber++;
                backtracking(s, i + 2, pointNumber);
                pointNumber--;
                s.erase(s.begin() + i + 1);
            }else break;
        }
    }
    vector<string> restoreIpAddresses(string s) {
        backtracking(s, 0, 0);
        return result;
    }
};
```

