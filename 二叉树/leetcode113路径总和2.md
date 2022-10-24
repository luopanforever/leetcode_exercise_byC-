#### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

**题目描述：**

 给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。 

**题解思路：**

利用回溯来遍历每一种情况，找到符合题目要求的路径，添加进vector<vector<int>>中。

定义一个全局的vector<vector<int>> result来存放结果

定义一个全局的vector<int> every来存放符合条件的路径

- 这个符合条件可以利用acommulate来对every进行求和，如果求和结果等于给定的条件则将该路径加入到result中

  - **部分代码：**

  - ```c++
    if(accumulate(every.begin(),every.end(),0) == count) result.push_back(every);
    ```

**递归三件套**

- 参数和返回值

  - 根节点和targetSum，targetSum用来判断某路径是否符合要求，需要访问整棵树才可以得出结果，不需要像leetcode112路径总和1那样找到了符合的就返回，所以这道题不需要返回值，只需要通过它来改变全局变量resul即可

- 终止条件

  - 当访问的是叶子节点的时候，来判断该路径是否符合条件

- 本层逻辑

  - 回溯就在这个地方

  - 部分代码：

    - ```c++
      if(root->left){
                  every.push_back(root->left->val);  //回溯
                  backTracking(root->left,count);
                  every.pop_back();  //回溯
              }    
              //右
              
              if(root->right){
                  every.push_back(root->right->val);  //回溯
                  backTracking(root->right,count);
                  every.pop_back();  //回溯
              }    
      ```

**完整代码：**

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> every;
    void backTracking(TreeNode * root, int count){
        if(!root->left && !root->right){
            if(accumulate(every.begin(),every.end(),0) == count) result.push_back(every);
            return;
        }
        //左
        
        if(root->left){
            every.push_back(root->left->val);
            backTracking(root->left,count);
            every.pop_back();
        }    
        //右
        
        if(root->right){
            every.push_back(root->right->val);
            backTracking(root->right,count);
            every.pop_back();
        }    
        
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        result.clear();
        every.clear();
        if(!root) return result;
        every.push_back(root->val);
        backTracking(root, targetSum);
        return result;
    }
};
```

