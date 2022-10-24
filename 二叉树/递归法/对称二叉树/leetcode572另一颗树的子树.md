#### leetcode572[ 另一棵树的子树](https://leetcode.cn/problems/subtree-of-another-tree/)

递归部分与leetcode100相同

```c++
bool compare(TreeNode * root, TreeNode * subRoot){
        if(!root&&subRoot) return false; 
        else if(root&&!subRoot) return false;
        else if(!root&&!subRoot) return true;
        else if(root->val != subRoot->val) return false;

        int l = compare(root->left, subRoot->left);
        int r = compare(root->right, subRoot->right);
        int result = l && r;
        return result;
    }
```

“主”函数部分需要稍做改动  , 利用迭代的前序遍历访问root中的每一个节点，让其跟subroot比较是否相同

```c++
bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        stack<TreeNode* >s;
        if(!root && !subRoot) return true;
        else if (!root && subRoot) return false;
        else s.push(root);
        while(!s.empty()){
            TreeNode * cur = s.top();
            if(compare(cur, subRoot)) return true;
            s.pop();
            if(cur->right) s.push(cur->right);
            if(cur->left) s.push(cur->left);
        }
        return false; 
    }
```

