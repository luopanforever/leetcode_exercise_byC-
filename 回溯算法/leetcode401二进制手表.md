#### [401. 二进制手表](https://leetcode.cn/problems/binary-watch/)

难度简单383

二进制手表顶部有 4 个 LED 代表 **小时（0-11）**，底部的 6 个 LED 代表 **分钟（0-59）**。每个 LED 代表一个 0 或 1，最低位在右侧。

- 例如，下面的二进制手表读取 `"3:25"` 。

![leetcode401](image\leetcode401.jpg)

*（图源：[WikiMedia - Binary clock samui moon.jpg](https://commons.m.wikimedia.org/wiki/File:Binary_clock_samui_moon.jpg) ，许可协议：[Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0)](https://creativecommons.org/licenses/by-sa/3.0/deed.en) ）*

给你一个整数 `turnedOn` ，表示当前亮着的 LED 的数量，返回二进制手表可以表示的所有可能时间。你可以 **按任意顺序** 返回答案。

小时不会以零开头：

- 例如，`"01:00"` 是无效的时间，正确的写法应该是 `"1:00"` 。

分钟必须由两位数组成，可能会以零开头：

- 例如，`"10:2"` 是无效的时间，正确的写法应该是 `"10:02"` 。

 

**示例 1：**

```
输入：turnedOn = 1
输出：["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]
```

**示例 2：**

```
输入：turnedOn = 9
输出：[]
```

**提示：**

- `0 <= turnedOn <= 10`

**题解思路：**

首先抛开字符串，抽象题目，从`10`个数字中选择`turnedOn`个数字，不能重复，就这么简单直接就可以用回溯枚举，伪代码如下

```c++
for(int i = startIndex; i < 10; i++){
    // 选择数字i
    backtracking();
    // 回溯数字i
}
```

这就是本题被扒光的情景，接下来就是根据题意一点一点的加限制条件

这道题被扒光了构造回溯抽象树太简单了，就不画了`hhh`

这十个数就是上图二进制手表上的数字，用一个`vector`容器来保存

- ```c++
  vector<int> time{1, 2, 4, 8, 1, 2, 4, 8, 16, 32};
  // 0-3代表小时，4-9代表分钟
  ```

先不管题目中要求返回字符串时间，我就用两个`int`型参数`hours`和`minites`来保存回溯提取的`time`，判断当前索引是在哪个区间就为哪个变量操作

- ```c++
  if(i >= 0 && i <= 3){ // 处理小时
      hours += time[i];
      // backtracking
      hours -= time[i];
  }else{                // 处理分钟
      minites += time[i];
      // backtracking...
      minites -= time[i];
  }
  ```

**递归三步**

- 确定参数和返回值
  - 遍历整棵回溯抽象树不需要返回值
  - 参数列表：
    - 需要一个`countIndex`来表示已经选择了多少个灯，用来参与终止条件
    - 选过的灯不能再次选择，所以需要一个`startIndex`，用于递归回溯同一节点的下一个节点

- 确定终止条件

  - 当`countIndex == turnedOn`的时候就可以开始处理返回了

  - 前面我们已经获得了两个变量，一个表示**小时**，一个表示**分钟**，要将其处理为`string`串，而且分钟需要格式化输出，不足两位的前面要补零，我用的方法是使用头文件`sstream`中的`ostringstream`和头文件`iomanip`中的`setw`和`setfill`，`int`转`string`用头文件`string`中的`to_string`函数

    ```c++
    if(countIndex == turnedOn){
        if(hours > 11 || minites > 59) return ;
        string hour = to_string(hours);
        ostringstream minite;
        minite << setw(2) << setfill('0') << minites;  // cout 是将数据输出到控制台， ostringstream是输出到其字符串流中  .str() 可以提取其中的字符
        string maoHao = ":";
        result.push_back(hour + maoHao + minite.str());
    }
    ```

- 确定本层逻辑
  - 前面已经分析

**完整代码**

```c++
class Solution {
public:
    vector<string> result;
    // countIndex当前层共使用了多少灯
    void backtracking(vector<int> time,int turnedOn, int countIndex, int startIndex, int hours, int minites){
        if(countIndex == turnedOn){
            if(hours > 11 || minites > 59) return ;
            string hour = to_string(hours);
            ostringstream minite;
            minite << setw(2) << setfill('0') << minites; 
            string maoHao = ":";
            result.push_back(hour + maoHao + minite.str());
        }
        for(int i = startIndex; i < time.size(); i++){
            if(i >= 0 && i <= 3){ // 处理小时
                hours += time[i];
                backtracking(time, turnedOn, countIndex + 1, i + 1, hours, minites);
                hours -= time[i];
            }else{                // 处理分钟
                minites += time[i];
                backtracking(time, turnedOn, countIndex + 1, i + 1, hours, minites);
                minites -= time[i];
            }
        }
    }
    vector<string> readBinaryWatch(int turnedOn) {
        vector<int> time{1,2,4,8,1,2,4,8,16,32};  // hours:0-3 minites:4-9
        backtracking(time,turnedOn, 0, 0, 0, 0);
        return result;
    }
};
```

