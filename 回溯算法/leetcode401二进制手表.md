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

。。。明天来写