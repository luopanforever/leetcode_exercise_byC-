#### [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

**题目描述**

给定一个不重复的整数数组 `nums `。 **最大二叉树** 可以用下面的算法从 `nums` 递归地构建:

- 创建一个根节点，其值为 `nums` 中的最大值。

-  递归地在最大值 **左边** 的 **子数组前缀上** 构建左子树。

- 递归地在最大值 **右边** 的 **子数组后缀上** 构建右子树。

返回 `nums` 构建的 **最大二叉树** 。

**题解思路**

- 我首先写了一个方法，是找最大值及其下标索引的方法，参数有数组和左右下标，根据下标给出的范围来找这个范围中的最大值，**代码如下**

  - ```c++
    pair<int,int> findmax(vector<int> nums, int low, int high){
            int temp = INT_MIN;
            int index = 0;
            for(int i = low; i < high; i++){
                if(temp < nums[i]) {
                    temp = nums[i];
                    index = i;
                } 
            }
            return {temp,index};
        }
    ```
    
  - INT_MIN放在**\<limit>**头文件中，其中还有INT_MAX可用

- 接下来就是递归的部分

  - 遇见递归就要想到三步：

    - 确定参数和返回值

      - 参数会放一个数组和前后下标，这道题递归的核心就是如何来分割数组，我就用下标来分割数组，返回值返回`TreeNode*`，这样**“主函数”**就可以来接收
      - `reeNode* travesal(vector<int> & nums, int low, int high);`

    - 确定终止条件

      - 终止条件有俩，一是一来就确定给定的范围是否为空
        - `if(high == low) return nullptr;`
      - 在范围不为空的情况下，在根节点建立后判断该节点是否为叶子节点，如果是，提前返回根节点，不用执行之后的分割操作
        - `if(high - low == 1) return root;`

    - 确定本层逻辑

      - 首先找到给定范围下的最大值，然后创建根节点

        - ```c++
          pair<int,int> maxAndindex= findmax(nums, low, high);
          TreeNode* root = new TreeNode(maxAndindex.first);
          ```

      - 然后利用最大值找到切割点并且进行切割操作

        - ```c++
          int sub = maxAndindex.second;
          
          int lowleft = low;
          int lowright = sub;
          
          int highleft = sub + 1;
          int highright = high;
          ```
        
      - 最后左右子树的构造就交给递归
      
        - ```c++
          root->left = travesal(nums, lowleft, lowright);
          root->right = travesal(nums, highleft, highright);
          ```

**完整代码**

```c++
class Solution {
public:
    pair<int,int> findmax(vector<int> nums, int low, int high){
        int temp = INT_MIN;
        int index = 0;
        for(int i = low; i < high; i++){
            if(temp < nums[i]) {
                temp = nums[i];
                index = i;
            } 
        }
        return {temp,index};
    }
    TreeNode* travesal(vector<int> & nums, int low, int high){
        if(high == low) return nullptr;
        pair<int,int> maxAndindex= findmax(nums, low, high);
        TreeNode* root = new TreeNode(maxAndindex.first);
        if(high - low == 1) return root;
        int sub = maxAndindex.second;

        int lowleft = low;
        int lowright = sub;

        int highleft = sub + 1;
        int highright = high;
    
        root->left = travesal(nums, lowleft, lowright);
        root->right = travesal(nums, highleft, highright);

        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return travesal(nums, 0, nums.size());
    }
};
```

 **简洁高效版本**

```c++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return buildTree(nums,0,nums.size());
    }
    TreeNode* buildTree(vector<int>& nums,int l, int r)
    {
        if(l == r) return nullptr;
        int maxVal = INT_MIN;
        int index = 0;
        for(int i = l ;i < r ; i++)
        {
            if(maxVal < nums[i])
            {
                maxVal = nums[i];
                index = i;
            }
        }
        TreeNode* head = new TreeNode(maxVal);
        if(r - l == 1) return head;
        head->left = buildTree(nums,l,index);
        head->right = buildTree(nums,index + 1,r);
        return head;
    }
};
```

**优雅版本**——**操作迭代器**

- 利用了头文件\<algorithm>的泛型算法max_element ，返回最大值代表的迭代器

```c++
class Solution {
public:
    TreeNode* travesal(vector<int> & nums, vector<int>::iterator left, vector<int>::iterator right){
        if(right == left) return nullptr;
        auto it = max_element(left, right);
        TreeNode* root = new TreeNode(*it);  //解引用就是值，本身就是位置
        if(right - left == 1) return root;        
    
        root->left = travesal(nums, left, it);
        root->right = travesal(nums, it + 1, right);

        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return travesal(nums, nums.begin(), nums.end());
    }
};
```

