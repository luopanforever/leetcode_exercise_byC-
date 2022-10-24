#### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

难度中等943

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

**题解思路**

对比`leetcode78`子集一题，在原数组中多了重复的数字，这就导致了子集一定有重复的，而题解要求不能有重复的，因此在回溯中需要用到一种方法，增加一个`used`数组，大小与`nums`数组相同，全体初始化为`false`，`used[i] == false`代表`num[i]`被枚举过，反之没有被枚举过。于是在回溯的外层for循环中可以加一个去重的条件，判定`i`与`i-1`索引位置处值相同且被使用过就`continue`掉这一次循环

```c++
if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
    continue;
}
```

其他地方与`leetcode78`相同，本题就是多了一个去重的步骤

**完整代码**

```c++
class Solution {
public:
    vector<vector<int> > result;
    vector<int> path;
    void backtracking(vector<int> nums, int startIndex, vector<bool> used){
        result.push_back(path);
        for(int i = startIndex; i < nums.size(); i++){
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            path.push_back(nums[i]);
            used[i] = true;
            backtracking(nums, i + 1, used);
            used[i] = false;
            path.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        backtracking(nums, 0, used);
        return result;
    }
};
```



