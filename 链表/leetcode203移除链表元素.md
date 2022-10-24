给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例 2：**

```
输入：head = [], val = 1
输出：[]
```

**示例 3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

 

**提示：**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`



解题思路：

为了便于第一节点的删除与其他节点的删除采用相同的方法，设置一个头结点指向题目所给链表的第一节点

利用一个移动指针遍历链表，查找到val值等于题目所给val时进行链表的删除操作

删除的核心代码` ptr->next = ptr->next->next;`

代码中使用temp指向ptr->next是为了方便释放删除节点的内存

```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *vitual_head = new ListNode(0);  //为链表设置一个头结点
        vitual_head->next = head;  //将头结点的下一个节点指针指向题目所给链表的第一个节点
        ListNode* ptr = vitual_head;  //设置一个移动指针  用于遍历链表  来删除指针
        while(ptr->next != NULL){
            if(ptr->next->val == val){
                ListNode *temp = ptr->next;
                ptr->next = temp->next;
                delete temp;
            }else{
                ptr = ptr->next;
            }
        }
        return vitual_head->next;

    }
};
```

