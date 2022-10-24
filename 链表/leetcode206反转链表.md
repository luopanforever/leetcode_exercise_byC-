给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```





**解题思路：**

逐个遍历节点，倒转next指针

代码如下：

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *pri_null = nullptr; //创建的指向null的指针
        while(head){  //逐个访问链表节点
            ListNode *temp = head; //建立临时指针保存head的信息防止丢失
            head = temp->next; 
            temp->next = pri_null;
            pri_null =temp;   
        }     
        return pri_null; 
    }
};
```

