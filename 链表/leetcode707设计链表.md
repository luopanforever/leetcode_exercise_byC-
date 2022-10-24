设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：`val` 和 `next`。`val` 是当前节点的值，`next` 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

- get(index)：获取链表中第 `index` 个节点的值。如果索引无效，则返回`-1`。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 `val` 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 `val` 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 `index` 个节点之前添加值为 `val` 的节点。如果 `index` 等于链表的长度，则该节点将附加到链表的末尾。如果 `index` 大于链表长度，则不会插入节点。如果`index`小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 `index` 有效，则删除链表中的第 `index` 个节点。

 

**示例：**

```
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
linkedList.get(1);            //返回2
linkedList.deleteAtIndex(1);  //现在链表是1-> 3
linkedList.get(1);            //返回3
```

 

**提示：**

- 所有`val`值都在 `[1, 1000]` 之内。
- 操作次数将在 `[1, 1000]` 之内。
- 请不要使用内置的 LinkedList 库



**解题思路：**

​	**独自解决的过程：**

​	写完之后通过了63个测试用例，还差最后一个，经过一系列的debug，最终找到自己写的代码的核心问题，在实现按指定位置插入节点的时候，当指定位置大于该链表的长度的时候还是会插入进链表。原始按指定位置插入代码如下

```c++
void addAtIndex(int index, int val) {
		MyLinkedList *new_node = new MyLinkedList();  //创建一个新节点，即将按照指定位置插入原链表
		//初始化新节点
		new_node->val = val;
		new_node->next = NULL;

		MyLinkedList *cur_node = this;
		int cur_index = 0;
		while (cur_node->next && cur_index <= index) {
			if (cur_index == index) {
				break;
			}
			else {
				cur_index++;
				cur_node = cur_node->next;
			}
		}
		new_node->next = cur_node->next;
		cur_node->next = new_node;
	}
```

​	

​	为该类添加一个新的属性，用来记录该链表的节点数

​	int size;(初始化为0)

​	于是在每个对节点个数操作内都加入对size的操作。

 	于是在addatindex函数内一开始增加一个判定，如果索引值超过了size则直接结束

修改后的代码如下:

```c++
void addAtIndex(int index, int val) {  //在指定位置插入数据时如果 索引超过了节点的个数则无法正常操作此函数
	if (this->size < index)
		return;
	MyLinkedList *new_node = new MyLinkedList();  //创建一个新节点，即将按照指定位置插入原链表
		//初始化新节点
	new_node->val = val;
	new_node->next = NULL;

	MyLinkedList *cur_node = this;
	int cur_index = 0;
	while (cur_index <= index) {
		if (cur_index == index) {
			new_node->next = cur_node->next;
			cur_node->next = new_node;
			this->size++;
			break;
		}
		else {
			cur_index++;
			cur_node = cur_node->next;
		}
	}
}
```

get函数的代码也许加一个判定，防止索引越界带来的访问错误

get代码如下：

```c++
int get(int index) {
	if (this->size <= index) return -1;
	int cur_index = 0;
	MyLinkedList *curprimer = this;  //可修改处
	while (cur_index <= index) {
		if (cur_index == index) {
			return curprimer->next->val;
		}
		else {
			cur_index++;
			curprimer = curprimer->next;
		}
	}
	return -1;
}
```





**源代码如下：**

```c++
class MyLinkedList {
public:
	MyLinkedList() {
		this->next = NULL;
		this->size = 0;
	}

	int get(int index) {
        if(this->size <= index) return -1;  // 洗个澡再看
		int cur_index = 0;
		MyLinkedList *curprimer = this;  
		while (cur_index <= index) {
			if (cur_index == index) {
				return curprimer->next->val;
			}
			else {
				cur_index++;
				curprimer = curprimer->next;
			}
		}
		return -1;
	}

	void addAtHead(int val) {
		MyLinkedList *new_node = new MyLinkedList(); //创建一个新节点，即将前插入原链表
		//初始化新节点
		new_node->val = val;
		new_node->next = NULL;
		//核心操作
		new_node->next = this->next;
		this->next = new_node;
		this->size++;
	}

	void addAtTail(int val) {
		MyLinkedList *new_node = new MyLinkedList();  //创建一个新节点，即将后插入原链表
		new_node->val = val;
		new_node->next = NULL;

		MyLinkedList *cur_node = this;
		//利用while循环将cur_node指向最后一个节点
		while (cur_node->next) {
			cur_node = cur_node->next;
		}
		new_node->next = cur_node->next;
		cur_node->next = new_node;
		this->size++;
	}

	void addAtIndex(int index, int val) {  //在指定位置插入数据时如果 索引超过了节点的个数则无法正常操作此函数
		if (this->size < index)
			return;
		MyLinkedList *new_node = new MyLinkedList();  //创建一个新节点，即将按照指定位置插入原链表
		//初始化新节点
		new_node->val = val;
		new_node->next = NULL;

		MyLinkedList *cur_node = this;
		int cur_index = 0;
		while (cur_index <= index) {
			if (cur_index == index) {
				new_node->next = cur_node->next;
				cur_node->next = new_node;
                this->size++;
				break;
			}
			else {
				cur_index++;
				cur_node = cur_node->next;
			}
		}
	}
	void deleteAtIndex(int index) {
		MyLinkedList *cur_node = this;
		int cur_index = 0;
		while (cur_node->next && cur_index <= index) {
			if (cur_index == index) {
				MyLinkedList *temp = cur_node->next;
				cur_node->next = temp->next;
				delete temp;
				this->size--;
				break;
			}
			else {
				cur_index++;
				cur_node = cur_node->next;
			}
		}
	}
	int val;
	int size;
	MyLinkedList* next;
};
```

