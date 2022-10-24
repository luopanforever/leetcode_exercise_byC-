#### [46. 全排列](https://leetcode.cn/problems/permutations/)

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**



#### **题解思路**

**设置全局数组**

```c++
vector<vector<int> > result;  // 保存结果
vector<int> path;  // 保存每一种情况
```



- 关于`startIndex`的确定
  - `nums`中的整数互不相同，无需考虑去重问题，本题求的是排列，不同与组合，在排列中`[1,2,3]`和`[3,2,1]`是不同的解，而在组合中是相同的解，这就确定了一个参数的有无，`startIndex`，其在组合中的作用是用来确定for循环的起始位置，目的是在回溯抽象树选取一个结点后子节点的索引值会往后移，就避免出现[1,2,3]和[1,3,2]，而在排列中无需`startIndex`，保证的是随着回溯抽象树的深度增，其子节点的个数都是一样的

- **关于同一树枝中不能使用相同结点的解决办法**
  
  - 在`leetcode491递增子序列`中，要避免同一层使用相同的数字，使用的是一个`backtracking`中的局部集合，且回溯前后无对应的`erase`操作，只有`insert`，而本题要使用一个全局集合，而本题是保证同一树枝路径下不会存在加入相同的数，就需要一个全局的集合来保存使用过的节点，然后还需要对应的`erase`操作
  
  - 也可以用全局`bool`数组，用过的节点就设置为`true`
  
    ```c++
    if (used[i] == true) continue;
    used[i] = true;
    ...
    backtracking(nums, used);
    ...
    used[i] = false;
    ```

**递归三步**

- 确定参数和返回值

  - 回溯一般没有返回值，此题也没有
  - `used`数组可以是全局的也可以是局部的，我将其设置为参数

- 确定终止条件

  - 全排列每个解的大小一定等于`nums`数组的大小，所以返回条件是只要`path`大小等于了`nums`，无需写`return`

    ```c++
    if(path.size() == nums.size()){
        result.push_back(path);
    }
    ```

- 确定本层逻辑

  - ```c++
    for(int i = 0; i < nums.size(); i++){
        if(used.find(nums[i]) != used.end()) continue;
        used.insert(nums[i]);
        path.push_back(nums[i]);
        backtracking(nums, used);
        path.pop_back();
        used.erase(nums[i]);
    }
    ```

#### **完整代码**

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> nums, unordered_set<int> used){
        if(path.size() == nums.size()){
            result.push_back(path);
        }
        for(int i = 0; i < nums.size(); i++){
            if(used.find(nums[i]) != used.end()) continue;
            used.insert(nums[i]);
            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used.erase(nums[i]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        unordered_set<int> used;  // 记录在同一树枝下是否使用过
        backtracking(nums, used);
        return result;
    }
};
```

