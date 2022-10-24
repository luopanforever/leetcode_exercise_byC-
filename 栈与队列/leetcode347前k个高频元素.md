**题目描述**

给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

**示例 1:**
```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```
**示例 2:**
```
输入: nums = [1], k = 1
输出: [1]
```

**提示：**

- `1 <= nums.length <= 105`
- `k 的取值范围是 [1, 数组中不相同的元素的个数]`
- `题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的`


进阶：你所设计算法的时间复杂度 必须 优于 O(n log n) ，其中 n 是数组大小。

**解题思路：**

关键字：优先级队列（小顶堆）、仿函数

利用map字典将各个字段的频率存储进去，之后利用在将map中的数据存入优先级队列即可，考虑到是求从高到低的元素，则利用小顶堆，则初始化优先级队列的时候应该使用>的仿函数来实现小顶堆。

**代码：**

```c++
class Solution {
public:
class mycomparison {//仿函数
    public:
        bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
            return lhs.second > rhs.second;
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> count; //统计频率
        for (int i = 0; i < nums.size(); i++) {
            count[nums[i]]++;
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;
        for(auto it = count.begin(); it != count.end(); it++){
            pri_que.push(*it);
            if(pri_que.size()>k){
                pri_que.pop();
            }
        }
        vector<int> result(k);
        for(int i = k - 1; i >= 0; i--){
            result[i] = pri_que.top().first;
            pri_que.pop();
        }
        return result;
    }
};
```

