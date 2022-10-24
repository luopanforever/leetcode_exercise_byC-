#### [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`



**题解思路：**

**设置全局数组**

```c++
vector<vector<int> > result;  // 保存结果
vector<int> path;  // 保存每一种情况
```

本题相较于全排列1多了序列里面可以包含重复数字的条件，这样一来全排列的解就会包含重复的解，就要求我们针对这个重复设计方法，

- **去除同一树枝中的相同结点**

  - 数组元素有重复的话就不能用全局的集合来去除同一树枝中的相同结点，由于这个方法是遍历到当前元素后将其加入集合，之后是通过`find`来查找然后`continue`，所以对于`[1,1,2]`，的解的大小永远无法超过三个，所以只能用全局`bool`数组，对使用过的标记为`true`

- **去除同一层上的相同结点**

  - 参照`leetcode491递增子序列`，使用局部集合来去掉同一层上的重复结点。

    ```c++
     unordered_set<int> usedAtNums;
    for(int i = 0; i < nums.size(); i++){
        if(usedAtNums.find(nums[i]) != usedAtNums.end() ||...) continue;
        ...
        usedAtNums.insert(nums[i]);
        backtracking(nums, usedAtBranch);
        ...
    }
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
    unordered_set<int> usedAtNums;
    for(int i = 0; i < nums.size(); i++){
        if(usedAtNums.find(nums[i]) != usedAtNums.end() || usedAtBranch[i] == true) continue;
        path.push_back(nums[i]);
        usedAtBranch[i] = true;
        usedAtNums.insert(nums[i]);
        backtracking(nums, usedAtBranch);
        path.pop_back();
        usedAtBranch[i] = false;
    }
    ```

#### 完整代码

```c++
class Solution {
public:
    vector<vector<int> > result;
    vector<int> path;
    void backtracking(vector<int> nums, vector<bool> usedAtBranch){
        if(path.size() == nums.size()){
            result.push_back(path);
        }
        unordered_set<int> usedAtNums;
        for(int i = 0; i < nums.size(); i++){
            if(usedAtNums.find(nums[i]) != usedAtNums.end() || usedAtBranch[i] == true) continue;
            path.push_back(nums[i]);
            usedAtBranch[i] = true;
            usedAtNums.insert(nums[i]);
            backtracking(nums, usedAtBranch);
            path.pop_back();
            usedAtBranch[i] = false;
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<bool> usedAtBranch(nums.size(), false);
        backtracking(nums, usedAtBranch);
        return result;
    }
};
```



