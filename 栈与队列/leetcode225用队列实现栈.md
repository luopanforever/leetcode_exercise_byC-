请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（`push`、`top`、`pop` 和 `empty`）。

实现 `MyStack` 类：

- `void push(int x)` 将元素 x 压入栈顶。
- `int pop()` 移除并返回栈顶元素。
- `int top()` 返回栈顶元素。
- `boolean empty()` 如果栈是空的，返回 `true` ；否则，返回 `false` 。

 

**注意：**

- 你只能使用队列的基本操作 —— 也就是 `push to back`、`peek/pop from front`、`size` 和 `is empty` 这些操作。
- 你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

**题解思路：**

两个队列相互配合，在进行每一步操作之前必定有一个队列是空的

- empty的实现：
  - 如果两个队列都是空 则返回true 否则返回false
- top的实现：
  - 返回非空队列的尾部元素
- push的实现：
  - 向非空队列push元素
  - 一开始两个队列都是空的的时候默认向一个队列push
- pop的实现（决定了为什么有一个队列必须为空）
  - 将非空队列中除最后一个元素外全部放入另一个空队列，然后返回之前的非空队列的头元素（仅剩这一个元素），在pop掉剩下的一个元素，然后该队列就为空了，以后就是先对另外一个队列进行操作

**代码：**

```c++
class MyStack {
public:
    MyStack() {
    }
    void push(int x) {
        if(this->empty()){
            queue1.push(x);
            return;
        }
        if(queue1.empty()){
            queue2.push(x);    
        }else{
            queue1.push(x);
        }
    }
    int pop() {
        //判断哪个队列为空就往哪个队列中注入全部数据
        if(queue1.empty()){
            while(queue2.size()!=1){
                queue1.push(queue2.front());
                queue2.pop();
            }
            int result = queue2.front();
            queue2.pop();
            return result;
        }else{
            while(queue1.size()!=1){
                queue2.push(queue1.front());
                queue1.pop();
            }
            int result = queue1.front();
            queue1.pop();
            return result;
        }
    }
    int top() {
        if(queue1.empty()){
            return queue2.back();
        }else{
            return queue1.back();
        }
    }
    bool empty() {
        if(queue1.empty()&&queue2.empty()) return true;
        return false;
    }
    queue<int> queue1;
    queue<int> queue2;
};
```

