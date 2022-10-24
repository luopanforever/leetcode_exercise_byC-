#### leetcode110[平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

只关注本层逻辑，不要想复杂了，有需要的就交给递归的用来给本层提供信息

```c++
class Solution {
public:
    int e_avg(TreeNode * root){
        if(!root) return 0;
        int left = e_avg(root->left);
        if(left == -1) return -1;
        int right = e_avg(root->right);
        if(right == -1) return -1;
        return abs(left - right) <= 1 ? 1 + max(left, right) : -1;
    }
    bool isBalanced(TreeNode* root) {
        return (e_avg(root) == -1)?false:true;
    }
};
```