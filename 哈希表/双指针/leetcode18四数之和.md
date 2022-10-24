给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

 

**示例 1：**

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**示例 2：**

```
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

 

**提示：**

- `1 <= nums.length <= 200`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`



**题目思路**

​	与三数之和的解法相同，都是利用双指针，只是多嵌套了一个for循环，这样一来有两个固定值，利用双指针操作数组

​	**去重：**

```c++
if(i>0 && nums[i] == nums[i-1])   //最前面的元素来过滤重复值
                continue;
```

​	这样做的目的是防止遗漏数据

例如：

​	输入数据为{2,2,2,2,2} 8

如果使用`nums[i] == nums[i+1]`

​	则第一索引会直接跳过全部数据  导致本来有一个[2,2,2,2]的解会被忽略

**代码：**

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        for(int i =0;i<nums.size();i++){

            if(i>0 && nums[i] == nums[i-1])   //最前面的元素来过滤重复值
                continue;
            
            for(int j =i+1;j<nums.size();j++){
                if(j>i+1 && nums[j] == nums[j-1])  //第二元素过滤重复值
                    continue;
                int left = j + 1;
                int right = nums.size() - 1;
                while(left < right){
                    if(nums[i] + nums[j] < target - (nums[left] + nums[right])){
                        left++;  
                        while(left<right&&nums[left] == nums[left - 1]) left++;  
                    }else if(nums[i] + nums[j] > target - (nums[left] + nums[right])){
                        right--;
                        while(left<right&&nums[right] == nums[right + 1]) right--;
                    }else{
                        result.push_back(vector<int>{nums[i],nums[j],nums[left],nums[right]});
                        while(left<right&&nums[left] == nums[left + 1]) left++;
                        while(left<right&&nums[right] == nums[right -1]) right--;
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```

