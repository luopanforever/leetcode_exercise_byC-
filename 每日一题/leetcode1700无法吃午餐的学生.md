#### [1700. 无法吃午餐的学生数量](https://leetcode.cn/problems/number-of-students-unable-to-eat-lunch/)

学校的自助午餐提供圆形和方形的三明治，分别用数字 `0` 和 `1` 表示。所有学生站在一个队列里，每个学生要么喜欢圆形的要么喜欢方形的。
餐厅里三明治的数量与学生的数量相同。所有三明治都放在一个 **栈** 里，每一轮：

- 如果队列最前面的学生 **喜欢** 栈顶的三明治，那么会 **拿走它** 并离开队列。
- 否则，这名学生会 **放弃这个三明治** 并回到队列的尾部。

这个过程会一直持续到队列里所有学生都不喜欢栈顶的三明治为止。

给你两个整数数组 `students` 和 `sandwiches` ，其中 `sandwiches[i]` 是栈里面第 `i` 个三明治的类型（`i = 0` 是栈的顶部）， `students[j]` 是初始队列里第 `j` 名学生对三明治的喜好（`j = 0` 是队列的最开始位置）。请你返回无法吃午餐的学生数量。

**示例 2：**

```
输入：students = [1,1,1,0,0,1], sandwiches = [1,0,0,0,1,1]
输出：3
```

 

**提示：**

- `1 <= students.length, sandwiches.length <= 100`
- `students.length == sandwiches.length`
- `sandwiches[i]` 要么是 `0` ，要么是 `1` 。
- `students[i]` 要么是 `0` ，要么是 `1` 。

**题解思路**

开始想使用双指针模拟这个过程

- 不匹配则学生指针++

- 匹配则学生指针++，三明治指针++，且设置学生值为2，用于标记吃过的学生

- 注意学生指针++要取模，这样可以模拟队列机制

  ` stu = ++stu % students.size();`

但是这个思路是对的，或者说我不知道后续怎么办，不知道如何结束这个循环，最终提交就是超时。

看评论发现一思路很妙，用一个大小为`2`的数组，题目中刚好有0和1，直接按照索引统计0和1，首先遍历学生数组，为数组增加元素

```c++
vector<int> result(2, 0);
for(auto i : students) result[0]++;  // 及其巧妙
```

然后因为取三明治只能从栈顶，然后遍历三明治，对于栈头元素，result[i] == 0 的话则可以直接返回，因为没人需要的三明治会一直在头那儿卡着，即使下面有其他学生喜欢的也不管

```c++
for(auto i : sandwiches){
    if( result[i] > 0 ) result[i]--;
    else break;
}
```

最后返回`result[0]`与`result[1`]即可

**完整代码**

```c++
class Solution {
public:
    int countStudents(vector<int>& students, vector<int>& sandwiches) {
        vector<int> result(2, 0);
        for(auto i : students){
            result[i]++;
        }
        for(auto i : sandwiches){
            if( result[i] > 0 ) result[i]--;
            else break;
        }
        return result[0] + result[1];
    }
};
```



