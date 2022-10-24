给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

 

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`



**解题思路：**

​	元素结果唯一且无序可以考虑unordered_set，该容器不会存储重复元素，且不会自动排序。可以将题目中任意一个数组装进这个unordered_set容器,然后利用其内置的find函数查找另一个数组的值，如果有查找到，则将其加入到另一个unordered_set容器里面即可。



**代码：**

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result;
        unordered_set<int> nums1_set;//用于去掉nums1中重复的元素 ，该容器相较于set是无序的
        for(int nums :nums1){
            nums1_set.insert(nums);
        }
        for(int nums:nums2){
            if(nums1_set.find(nums)!=nums1_set.end()){
                result.insert(nums);
            }
        }
        vector<int> r(result.begin(),result.end());
        return r;
    }
};
```

