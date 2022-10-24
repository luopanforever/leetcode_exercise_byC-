#### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

**题目描述**

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**题解思路**

本题我用了三种方法解题，分别是数组法，递归法，迭代法，他们的核心思想都是利用了二叉搜索树的中序遍历是一个递增的序列

- 数组法，利用中序遍历，将每个节点的值放入一个数组，然后判断这个数组是否是递增的即可

  - ```c++
    class Solution {
    public:
        void inorder(TreeNode* root, vector<int> &v){
            if(!root) return;
            inorder(root->left, v);
            v.push_back(root->val);
            inorder(root->right, v);
        }
        bool isBST(vector<int> v){
            for(int i = 0; i < v.size() - 1; i++){
                if(v[i] >= v[i+1]) return false;  // 如果有一个不是升序则返回false
            }
            return true;
        }
        bool isValidBST(TreeNode* root) {
            vector<int> v;
            inorder(root, v);  // 中序遍历将节点放入数组
            return isBST(v);  // 判断数组是否是升序
        }
    };
    ```

- 递归法，整体代码神似中序的递归

  - ```c++
    class Solution {
    public:
        long maxval = LONG_MIN;
        bool isbst(TreeNode* root){
            if(!root) return true;
    
            bool left = isbst(root->left);
    
            if(maxval < root->val) maxval = root->val;
            else return false;
    
            bool right = isbst(root->right);
    
            return left && right;
        }
        bool isValidBST(TreeNode* root) {
            return isbst(root);
        }
    };
    ```

  - 使用LONG_MIN而不使用INT_MIN的原因是有一个**测试用例**是`[-2147483648]`,也就是INT_MIN，而在leetcode中LONG_MIN是-2^64，是比这个测试用例小的，所以替换成LONG_MIN可以

- 迭代法，其实就是二叉树中序遍历的中序迭代遍历，这里我用的是统一迭代遍历法的中序遍历

  - ```c++
    lass Solution {
    public:
        long maxval = LONG_MIN;
        bool isValidBST(TreeNode* root) {
            stack<TreeNode*> s;
            if(!root) return true;
            s.push(root);
            while(!s.empty()){
                TreeNode *cur = s.top();s.pop();
                if(cur){
                    if(cur->right) s.push(cur->right);
                    s.push(cur);
                    s.push(NULL);
                    if(cur->left) s.push(cur->left); 
                }else{
                    cur = s.top();
                    s.pop();
                    if(maxval < cur->val) maxval = cur->val;
                    else return false;
                }
            }
            return true;
        }
    };
    ```

    