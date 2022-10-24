给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`





**题目思路：**

​	在独自做这道题的时候我有一个疑问，就是两个节点进行交换之后`first->next = second->next; second->next=first`second节点的信息会丢失，那么怎么保存它呢，看了carl的题解之后，知道了虚拟结点的重要性，可以设置一个虚拟头结点，在每次访问节点的时候让头结点的下一个节点指向第二个节点，从解决问题

**代码：**

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *first = new ListNode();  //虚拟头结点  不会动
        first->next = head;
        ListNode *cur = first;  //拷贝一个虚拟头结点 用于移动
            while(cur->next&&cur->next->next){
            ListNode *tem = cur->next;  //保存第一节点
            ListNode * temp = cur->next->next->next; //保存即将跳转的节点

            cur->next = cur->next->next;  //将“头”结点 指向第二节点
            cur->next->next = tem;  //将第二节点指向第一节点
            cur->next->next->next = temp;  //将第一节点指向第三节点

            cur = cur->next->next;  //将“头”结点 转移至下一个头结点的位置
        }
        return first->next;
    }
};
```

