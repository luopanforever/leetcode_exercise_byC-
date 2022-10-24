给你一个包含 `n` 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

**示例 2：**

```
输入：nums = []
输出：[]
```

**示例 3：**

```
输入：nums = [0]
输出：[]
```

 

**提示：**

- `0 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`





**题目思路：**

利用一个循环指针和双指针来对这个数组进行操作，在一个大的for循环内，i从数组头访问到数组尾，每次循环，头指针定义在i的后一个位置上，尾指针定义在数组尾。每次的for遍历将对收尾指针进行移动，由于不可以包括重复元组，故在每次移动指针时要查看其指向的值是否与上一次指向的值相等，如果相等则继续移动指针直到不相等。



**代码：**

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        for(int i =0;i<nums.size();i++){
            if(nums[i]>0){
                return result;
            }
            if(i>0 &&nums[i]==nums[i-1]){
                continue;
            }
            int left = i+1;
            int right = nums.size()-1;
            while(left<right){
                if(nums[i]+nums[left]+nums[right]>0){
                    right--;
                    while(right>left&&nums[right]==nums[right+1]) right--;
                    
                }else if(nums[i]+nums[left]+nums[right]<0){
                    left++;
                    while(right>left&&nums[left]==nums[left-1]) left++;
                }else{
                    result.push_back(vector<int>{nums[i],nums[left],nums[right]});
                    while(right>left&&nums[left]==nums[left+1]) left++;
                    while(right>left&&nums[right]==nums[right-1]) right--;
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
};
```

