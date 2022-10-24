#### leetcode111二叉树的最小深度

- 叶子节点
  - 左右子树都为空
- 最小深度
  - 根节点到最近的叶子节点的距离上的节点数
- 故下图的最小深度不是1、是5

![image-20220601093722733](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220601093722733.png)

```c++
class Solution {
public:
    //递归的理解提升了..  真的只用管本层逻辑
    int mindeep(TreeNode*node){
        if(!node) return 0;
        int left = mindeep(node->left);
        int right = mindeep(node->right);
        if(!left&&right) return 1 + right;
        if(left&&!right) return 1 + left;
        return 1 + min(left,right);
    }
    int minDepth(TreeNode* root) {
        return mindeep(root);
    }
};
```

