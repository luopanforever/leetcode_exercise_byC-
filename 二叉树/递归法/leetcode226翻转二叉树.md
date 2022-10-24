#### leetcode226[翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

**递归法**

前序递归法与后续递归法都可以，只要保证访问左右节点是挨着的就行，如果不挨着例如中序递归法，则会发生两次递归访问的都是左节点

```c++
class Solution {
public:
    void preOrder(TreeNode * root){
        if(!root) return;
        swap(root->left, root->right);
        preOrder(root->left);
        preOrder(root->right);
        
    }
    TreeNode* invertTree(TreeNode* root) {
        preOrder(root);
        return root;
    }
};
```