给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

 

**思路题解：**

​	独自解决的时候我是这样想的，遍历链表找到入口节点（想保存每一个节点信息作比较）。然后做了一会儿发现并不现实。于是去看了carl的解答，利用了一点数学知识和双指针的知识。

​	设置一个快指针和一个慢指针，如果链表有环的话则他们一定会在某一个节点相遇，并且这个节点肯定在环里面。于是将一条链表分为了三段距离；

第一段是慢指针到环入口的位置  设为x，

第二段是环入口到快慢指针相遇的位置  设为y，

第三段是快慢指针相遇位置到环入 设为z。

我就直接写结论x = z

详细证明可以看carl的题解：

[代码随想录 (programmercarl.com)](https://programmercarl.com/0142.环形链表II.html#思路)

利用这个结论，如果起点位置与相遇位置一同前行，则相遇位置就是入口位置。

首先定义快慢指针，找到他俩指向同一指针的时刻，此时快慢指针的位置 距离入口的位置就是z，此时让相遇位置与起点位置一同前行，找到相遇的位置，这就是入口位置。

**代码如下**

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head;
        ListNode *slow = head;
        if(fast == NULL) return NULL;
        while(fast->next&&fast->next->next){
            fast = fast->next->next;
            slow = slow->next;
            if(slow == fast){
                ListNode *index1 = slow;
                ListNode *index2 = head;
                while(index1!=index2){
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index1;
            }
        }
        return NULL;
    }
};
```



