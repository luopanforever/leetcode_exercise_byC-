#### [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

#### **题目描述**

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

**提示：**

- 树中节点的数目范围是 `[2, 104]`
- `0 <= Node.val <= 105`

#### 题解思路

因为数目节点是从2开始的，所以一开始不需要判空条件

可以用两种解法解决这道题，核心都是利用二叉树的中序是递增的数组

- 中序遍历构造一个递增数组，然后对这个数组进行操作来获取最小绝对差
- 中序遍历该树，每次遍历的时候都记录上一个访问节点，然后动态的改变`diff`

**递增数组法**

首先中序遍历， 我这里用迭代的中序遍历，是统一迭代遍历的中序遍历

```c++
int getMinimumDifference(TreeNode* root) {
    stack<TreeNode*> s;
    vector<int> v;
    int diff = 1000000;
    s.push(root);
    while(!s.empty()){
        TreeNode* cur = s.top(); 
        s.pop();
        if(cur){
            if(cur->right) s.push(cur->right);
            s.push(cur);
            s.push(nullptr);
            if(cur->left) s.push(cur->left);
        }else{
            cur = s.top(); s.pop();
            v.push_back(cur->val);
        }
    }
}
```

然后就是对数组进行操作

```c++
for(int i = 0; i < v.size() - 1; i++){
    diff = diff < v[i + 1] - v[i]? diff :v[i + 1] - v[i];
        }
```

最后返回`diff`即可

**记录上一节点法**

- 递归

  - 定义全局的`TreeNode* postnode;`变量，其会被值初始化为`nullptr`

  - 核心代码  （写在中序遍历访问中节点的下面）

    - ```c++
      if(postnode){
          diff = diff > (root->val - postnode->val) ? (root->val - postnode->val) : diff;
      }            
      postnode = root;
      ```

  - 完整代码

  - ```c++
    class Solution {
    public:  
        TreeNode *postnode;
        int diff = INT_MAX;
        void inorder(TreeNode * root){
            if(!root) return;
            inorder(root->left);
            if(postnode){
                diff = diff > (root->val - postnode->val) ? (root->val - postnode->val) : diff;
            }    
            postnode = root;
            inorder(root->right);
        }    
        int getMinimumDifference(TreeNode* root) {
            inorder(root);
            return diff;
        }    
    };       
    ```

- 迭代

  - 其实是一样的，就是加一个存储前一节点的逻辑即可

  - 完整代码

  - ```c++
    class Solution {
    public:
        TreeNode *postnode;
        int diff = INT_MAX;
        
        int getMinimumDifference(TreeNode* root) {
            stack<TreeNode*> s;
            s.push(root);
            while(!s.empty()){
                TreeNode* cur = s.top();
                s.pop();
                if(cur != nullptr){
                    if(cur->right) s.push(cur->right);
                    s.push(cur);
                    s.push(nullptr);
                    if(cur->left) s.push(cur->left);
                }else{
                    cur = s.top();
                    s.pop();
                    if(postnode) diff = min(diff, cur->val - postnode->val);
                    postnode = cur;
                }
            }
            return diff;
        }
    };
    ```

