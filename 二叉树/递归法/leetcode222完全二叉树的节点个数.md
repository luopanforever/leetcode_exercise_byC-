#### leetcode222

只管本层逻辑

访问到了某一节点，非空就返回该节点的个数1+左右子树的节点

**tips：**递归左右子节点就可以把他们想象成左右子树

```c++
class Solution {
public:
    int counts(TreeNode*root){
        if(!root) return 0;
        return 1 + counts(root->left) + counts(root->right);
    }
    int countNodes(TreeNode* root) {
        return counts(root);
    }
};
```

