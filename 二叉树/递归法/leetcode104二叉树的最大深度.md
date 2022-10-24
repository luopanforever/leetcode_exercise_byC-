#### leetcode104[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```c++
class Solution {
public:
    
    int deep(TreeNode*node){
        if(!node) return 0;
        int left = deep(node->left);
        int right = deep(node->right);
        return 1+max(left,right);
    }
    int maxDepth(TreeNode* root) {
        return deep(root);
    }
};
```

