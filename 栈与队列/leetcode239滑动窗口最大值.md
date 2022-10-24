给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`



**题解思路**

本题一开始直接想到的思路就是将nums分块，每个块各自排序然后找到最大值然后push_back到结果数组里面。思路是对的 但是太暴力了，超时是必然的，于是我从carl那里了解到可以用单调队列来解此题

- 单调队列

  - 此队列中的元素保持单调性，push进元素只能是比队列中已有元素大或者小，如果不满足，则会抹去队列中不满足的元素，知道可以push为止，这样就保持了队列的单调性

    - 例如：假设有一个单调队列，里面的元素是单调递减的，意思就是front永远是最大的，假设有一个数组[1, 3, -1, -3, 5, 3, 6, 7]，依次将里面的元素push_back进单调队列，队列元素的变化情况是

      [1]						首先push第一个元素

      [3]						push第二个元素的时候就要开始维护了，1<3，则抹去1，push  3  使第一元素保持最大

      [3,-1]					3>-1，直接push，不需要维护

      [3,-1,-3]				-1>-3，直接push，不需要维护

      [5]						any[3,-1,-3]<5 ,则直接抹去队列中全部元素然后push 5

      [5,3]					 5>3，直接push，不需要维护

      [6]						any[5,3]<6，则抹去队列中全部元素然后push 5

      [7]						6<7，则抹去6，push 7

    - 可以看出，单调队列中的元素一直保持着单调性，这就是单调队列

此题是找滑动窗口中的最大值，故单调递减队列可以满足（队头最大），滑动窗口的capacity为k，push操作很好理解和实现，这道题的关键是什么时候pop出第一元素，要保证窗口里面的元素都是窗口里面的元素（这句话看似是废话其实就是解题的关键，就是如果该窗口向后移动了，则必须pop掉不属于该窗口的值，有一种情况需要处理就是当窗口在这里时[  1    [  3    1    2  ]    0    5  ]，此时单调队列中的元素为[3, 2]，之后就是push 0 ，如何保证在capacity没满的情况下push 0且pop 3 是最关键的点），需要特殊设计pop函数，pop会传进一个value值，判断传进来的value值是否等于单调队列的队头值，该判断的意义是，需要结合调用该代码的部分进行说明

**部分代码：**

```c++
class my_que{
public:
...		
    	
        void pop_deque(int value){
            if(value == get_front()){
                d.pop_front();
            }
        } 
        int get_front(){ return d.front();}
vector<int> maxSlidingWindow(vector<int>& nums, int k) {  //假设nums:[1,3,1,2,0,5];k:3
... 
my_que temp;   
...
for(int i = k;i < nums.size();i++){
            temp.pop_deque(nums[i-k]);  //理解这一行代码的意义
            temp.push_deque(nums[i]);
            result.push_back(temp.get_front());
        }
...
```

nums[i-k] i-k的取值为 0,1,2，为0的意思是即将要push索引为3的值（2）了，在pop_deque函数中，会判断nums[0]是否等于get_front()，该判断的意义是查看索引为0的1值是否是初始滑动窗口的头值，如果是则会pop，这样就保证了窗口里面的元素都是窗口里面的元素。

难点已经解决，接下来是该程序的大致思路流程：

建立一个自己的单调队列，内部用deque实现，push的实现

```c++
//根据单调递减队列的原理所设计
void push_deque(int value){
        while(!d.empty() && d.back() < value)
            d.pop_back();
        d.push_back(value);
    }
```

**单调队列整体类代码展示**

```c++
class my_que{
private:
    deque<int> d;
public:
    my_que() = default;
    void push_deque(int value){
        while(!d.empty() && d.back() < value)
            d.pop_back();
        d.push_back(value);
    }
    void pop_deque(int value){
        if(value == get_front()){
            d.pop_front();
        }
    } 
    int get_front(){
        int result = d.front();
        return result;
    }
}; 
```

单调队列设计好之后，接下来就是窗口滑动模拟，以nums:[1,3,1,2,0,5];k:3来模拟，首先将nums的前k个元素push进单调队列，然后直接将第一元素push进result数组，接着开始移动窗口

**完整代码：**

```c++
class Solution {
public:
    class my_que{
    private:
        deque<int> d;
    public:
        void push_deque(int value){
            while(!d.empty() && d.back() < value) //如果不满足单调则从后向前逐个抹掉元素
                d.pop_back();
            d.push_back(value);
        }
        void pop_deque(int value){
            if(value == get_front()){
                d.pop_front();
            }
        } 
        int get_front(){
            int result = d.front();
            return result;
        }
    };
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        my_que temp;
        for(int i = 0;i < k;i++){
            temp.push_deque(nums[i]);
        }
        vector<int> result;
        result.push_back(temp.get_front());
        //开始滑动窗口
        for(int i = k;i < nums.size();i++){
            temp.pop_deque(nums[i-k]);
            temp.push_deque(nums[i]);
            result.push_back(temp.get_front());
        }
        return result;
    }
};
```

