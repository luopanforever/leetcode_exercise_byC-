给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://img-blog.csdnimg.cn/img_convert/8660b281e0a5bac0ef981fab90aa8e20.png)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`





**题目思路：**

​	本题可以用双指针解决，思路为设置一个快指针和一个慢指针，快指针先走到从正数的第n个位置，然后再让快慢指针一起向前next，最终当快指针==NULL时，慢指针就到了该删除节点的位置了

**代码：**

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *first = new ListNode();
        first->next = head;
        int i =0;
        ListNode *fast_ptr = first; //快指针
        ListNode *low_ptr = first; //慢指针
        ListNode *front_low_ptr = new ListNode();//用于保存慢指针的前一个指针 便于删除操作
        front_low_ptr->next =first;
        while(i<n){
            fast_ptr = fast_ptr->next;
            i++;
        }
        while(fast_ptr){
            fast_ptr = fast_ptr->next;
            low_ptr = low_ptr->next;
            front_low_ptr = front_low_ptr->next;
        }
        front_low_ptr->next = front_low_ptr->next->next;
        delete low_ptr;
        return first->next;
    }
};
```
**优化：**
在快指针指向n后，让快指针再往后走一步，这样可以使慢指针到最后指向最终节点的前一个节点，此优化可以省去前面定义的front_low_ptr，但是删除后面的操作需要定义一个临时变量保存删除节点的值  
