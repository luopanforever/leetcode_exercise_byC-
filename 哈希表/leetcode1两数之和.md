给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

**解题思路：**

要求数组中某两个数相加等于目标值，可以理解为建立一个hash表，将数组的值和下标存入该hash表，在每次便利的时候查找target-nums[i] 是否存在与hash表中，存在的话就返回下标，不存在的话就将nums[i]动态放入hash表中增加记录。 一开始hash表是空的，随着i的增加，如果一直没找到，则hash表中记录也越来越多，到最后一个记录的时候，如果还没找到target减去当前nums1的值时则返回空{}



**代码段：**

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> nums_map;
        for(int i = 0;i<nums.size();i++){
            unordered_map<int,int>::iterator keys = nums_map.find(target - nums[i]);
            if(keys != nums_map.end()){
                return {keys->second,i};
            }
            //四种插入方式
          //nums_map.insert(pair<int,int>(nums[i],i));
          //nums_map[nums[i]] = i;
          //nums_map.insert(make_pair(nums[i],i));
          nums_map.insert(unordered_map<int,int>::value_type(nums[i],i));
        }
        return {};
    }
};
```

