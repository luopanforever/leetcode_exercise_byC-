#### 为什么写这个内容

之前刷leetcode第一题的时候是用的hash表法做的，后来做三数之和与四数之和的时候强化了一下自己对于双指针的理解，于是想用双指针来重新做leetcode这一道题

一开始的思路

```c++
vector<int> twoSum(vector<int>& nums, int target) {
	sort(nums.begin(),nums.end());					
    int left = 0;									//指向数组头
    int right = nums.size() - 1;					//指向数组尾
    while(left<right){ 								//当两个指针对撞结束循环
        if(nums[left] + nums[right] > target){		//大于目标值就左移右指针
            right--;
        }else if(nums[left] + nums[right] < target){//小于目标值就右移左指针
            left--;
        }else{
            return vector<int>{left,right};   //等于目标值时，返回左右指针  其实写这个的时候已经感觉有点不对劲了
        }
    }
    return {};
}

```

**测试**

测试一下：nums = [2,7,11,15]   target = 9

输出正确

然后自信的提交代码。。。

解答错误，查看错误，

发现如果nums本来无序的话，则我这个方法不行

**分析原因：**

看了下题目，对比了三数之和、四数之和与两数之和的区别，发现他俩因为是返回的值，所以使用sort标准库函数排序是不影响输出结果的，而这道题两数之和，是返回其值的索引值，故对nums排序是大错特错的，估计我是做三数之和与四数之和做魔怔了

**改正：**

由于两数之和是返回索引值，故不能动数组本身的相对位置，于是我想到了map容器，让key保存值，value保存索引值，这样一来，当将nums中的数据插入map时，又可以自动给nums的数据排序，又可以记录nums里面数据的索引值，由于nums中的数据可以重复，故我使用multimap（唯一可以重复key的map类型容器）。

**代码：**

```c++
vector<int> twoSum(vector<int>& nums, int target) {
    multimap<int,int> temp;  								//用来存放nums的值和索引值
    int index = 0;											//记录索引值			
    for(auto val : nums){
        temp.insert(pair<int,int>(val,index++));			//插入到multimap中自动排序的操作			
    }
    multimap<int,int>::iterator left = temp.begin();		//将双指针定义为迭代器
    multimap<int,int>::iterator right = --temp.end();		
    while(left != right){									//不可以写成left<right 该迭代器没有定义此操作
        if(left->first + right->first > target){			//后面的思路就与上面的思路一样
            right--;
        }else if(left->first + right->first < target){
            left++;
        }else{
            return vector<int>{left->second,right->second}; //返回索引值
        }
    }
    return vector<int>{};
}
```

**收获：**

之前觉得双指针有点抽象，但经过了三数之和、四数之和、还有自己重新用双指针解决两数之和后脑子里面对双指针的思路变得清晰了起来，并且对map的操作更熟悉了