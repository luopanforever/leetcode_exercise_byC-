给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

 

**示例 1：**

```
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**示例 2：**

```
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

 

 **提示：**

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`



**题解思路：**

​	我第一反应使用暴力算法，结果超时根本通过不了，于是利用可以利用hash表  将嵌套的四层for循环拆分为两个嵌套的双层for循环来降低时间复杂度,利用map建立hash表，key保存nums1+nums2的值，value保存该值出现的次数。第二层循环查找是否存在“已有值”与nums3+nums4相加为0的值，如果存在 则输出次数加value出现次数，具体实现方法为map.find( 0 -(num3+num4)); 



**代码：**

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        map<int,int> result;
        for(auto nums_1 : nums1){
            for(auto nums_2 : nums2){
                result[nums_1+nums_2]++;
            }
        }
        int sum_result = 0;
        for(auto nums_3 : nums3){
            for(auto nums_4 : nums4){
                map<int,int>::iterator it = result.find(0-(nums_3+nums_4));
                if(it != result.end()){
                    sum_result+=it->second;
                }
            }
        }
        return sum_result;
    }
};
```

