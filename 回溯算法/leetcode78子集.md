#### [78. 子集](https://leetcode.cn/problems/subsets/)

难度中等1818

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```



**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

#### **题解思路：**

利用回溯进行组合，外层for循环遍历宽度，递归遍历深度，输入样例是[1, 2, 3]，子集不能有重复的元素，所以要在函数参数中加入`startIndex`来作为深度中的起始位置，每次进入递归都要( + 1)，

**递归三步**

- 确定参数和返回类型
  - 参数就是数组与`startIndex`
  - 不需要返回值
- 确定终止条件
  - 求子集问题就是要遍历整个一颗树，可以无终止条件
- 确定本层逻辑
  - 一开始直接无条件向`result`中加入路径，然后执行回溯即可

#### **完整代码：**

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex){
        result.push_back(path);
        for(int i = startIndex; i < nums.size(); i++){
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
    }
};
```

