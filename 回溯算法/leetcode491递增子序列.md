#### [491. 递增子序列](https://leetcode.cn/problems/increasing-subsequences/)

给你一个整数数组 `nums` ，找出并返回所有该数组中不同的递增子序列，递增子序列中 **至少有两个元素** 。你可以按 **任意顺序** 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

**示例 1：**

```
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**示例 2：**

```
输入：nums = [4,4,3,2,1]
输出：[[4,4]]
```

**提示：**

- `1 <= nums.length <= 15`
- `-100 <= nums[i] <= 100`

#### 题解思路

此题类似于`leetcode90子集2`，本题就是在子集不重复的情况还要递增。

同样的设置两个全局变量，一个代表最终结果，一个代表每一步的

```c++
vector<vector<int>> result;
vector<int> path;
```

**递增条件的判断：**

- `path`集合要不为空，且`path`尾要小于当前要加入`path`的数

  ```c++
  !path.empty() && nums[i] < path.back()
  ```

#### **去重条件判断：**

- 本题的去重方法与`leetcode90`不同，这道题的序列是固定的，不能用`sort`提前排序，因此`bool`型的数组去重方式也就实效了，此处可以使用一个int类型的`set`集合，去重的核心思想是回溯抽象树的同一节点下的子节点的值不能相同。于是可以在`for`循环外定义一个`set`数组，标记已使用过的子节点，由于`set`数组中值的顺序没有要求，所以可以用`unordered_set`来优化一点效率，之后只要在`used`数组中查找到了当前`nums[i]`值，就会跳过这一子节点

  ```c++
  used.find(nums[i]) != used.end()
  ```

**部分去重代码**

```c++
set<int> used;
for(int i = startIndex; i < nums.size(); i++){
    if((!path.empty() && nums[i] < path.back()) 
      		|| used.find(nums[i]) != used.end()){
        used.insert(nums[i]);
        // ...
        backtracking(nums, i + 1);  // 
        // ...
        // 后续无需有对应的删除操作 used是局部数组 意在轮到父亲节点的时候创建
    }
}
```

#### **递归三步**

- 确定参数和返回值

  - 回溯一般没有返回值，此题只改变全局数组，无需返回值
  - 由于同一元素使用之后不能再次使用，所以需要一个`startIndex`，来传给递归函数访问下一子节点

- 确定终止条件

  - 题目要求子集的`size`最小`2`，所以只要path集合内元素大于1就可以将`path`加入`result`了

- 确定本层逻辑

  - ```c++
    for(int i = startIndex; i < nums.size(); i++){
        if((!path.empty() && nums[i] < path.back()) || used.find(nums[i]) != used.end()) continue;
        used.insert(nums[i]);
        path.push_back(nums[i]);
        backtracking(nums, i + 1);
        path.pop_back();
    }
    ```

#### **完整代码**

```c++
class Solution {
public:
    vector<vector<int> > result;
    vector<int> path;
    void backtracking(vector<int> &nums, int startIndex){
        if(path.size() >= 2){
            result.push_back(path);
        } 
        unordered_set<int> used;
        for(int i = startIndex; i < nums.size(); i++){
            if((!path.empty() && nums[i] < path.back()) || used.find(nums[i]) != used.end()) continue;
            used.insert(nums[i]);
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {

        backtracking(nums, 0);
        return result;
    }
};
```

