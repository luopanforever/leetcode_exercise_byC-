#### [112. 路径总和](https://leetcode.cn/problems/path-sum/)

**题目描述**

给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**题解思路**-----正在艰难的学习回溯。。。

- 递归法
- 包含了回溯，回溯我现在的理解就是可以访问未知输入的每一种情况
  - 本题就是利用回溯来枚举每一种情况，查看是否符合题目要求
- 思路是设置一个count来等于targetSum，然后每搜索到一个节点就将count减去该节点的值，如果到了叶子节点，count为0，则返回false

**递归三件套**

- 确定参数与返回值

  - 参数为根节点和count，返回一个bool值

- 确定终止条件

  - 当该节点是叶子节点且count降为了0返回true
  - 当该节点是叶子节点且count不为0返回false

- 确定本层逻辑

  - 递归函数是有返回值的，如果递归函数返回true，说明找到了合适的路径，应该立刻返回。

  - 回溯的部分也是在这里

  - ```c++
    if(root->left){
        count -= root->left->val;  //回溯
        if(backTravaling(root->left,count)) return true;
       	count += root->left->val;  //回溯
    }
            
    if(root->right){
     	count -= root->right->val;  //回溯
        if(backTravaling(root->right,count)) return true;
        count += root->right->val;	//回溯
    }
    ```

  - **简化版**

  - ```c++
    if(root->left) 
        if(backTracking(root->left, count - root->left->val)) return true;
    if(root->right)
        if(backTracking(root->right, count - root->right->val)) return true;
    ```

**完整代码：**

```c++
class Solution {
public:
    bool backTravaling(TreeNode * root, int count){
        if(!root->left&&!root->right&& count == 0) return true;  //中
        if(!root->left&&!root->right) return false;
        
        if(root->left){  //左
            count -= root->left->val;
            if(backTravaling(root->left,count)) return true;
            count += root->left->val;
        }
        
        if(root->right){  //右
            count -= root->right->val;
            if(backTravaling(root->right,count)) return true;
            count += root->right->val;
        }
        
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(!root) return false;
        int sum = targetSum;
        return backTravaling(root,targetSum - root->val);
    }
};
```



