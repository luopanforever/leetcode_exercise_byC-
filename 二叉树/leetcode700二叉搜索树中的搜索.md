#### [700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

**题目描述**

给定二叉搜索树（BST）的根节点 `root` 和一个整数值 `val`。

你需要在 `BST` 中找到节点值等于 `val` 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 `null` 。

**题解思路**

第一反应想出来的是用遍历来做，做一半发现是搜索树哈哈就加了几个条件

```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        stack<TreeNode*> s;
        if(!root) return nullptr;
        s.push(root);
        while(!s.empty()){
            TreeNode* cur = s.top(); s.pop();
            if(cur->val == val) return cur;
            int flag = cur->val > val ? 1 : 0;
            if(cur->left&&flag) s.push(cur->left);
            if(cur->right&&!flag) s.push(cur->right);
            if(!cur->left&&flag || !cur->right&&!flag) return nullptr;
        }
        return nullptr;
    }
};
```

过于麻烦了

完全利用二叉搜索树的性质的话就是这样很简洁

```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root){
            if(root->val > val) root = root->left;
            else if(root->val < val) root = root->right;
            else return root; 
        }
        return nullptr;
    }
};
```

**递归**也是很简单这道题

```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if(!root) return root;
        if(root->val == val) return root;
        if(root->val > val) return searchBST(root->left, val);
        if(root->val < val) return searchBST(root->right, val);
        return nullptr;
    }
};
```

