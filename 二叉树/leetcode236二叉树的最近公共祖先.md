#### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

**题解思路**

递归+回溯

递归三步

- 确定参数和返回值
  - 返回值返回`TreeNode*`方便返回结果与递归，参数就是本身，则直接用给出的函数即可
- 确定终止条件
  - 当后序遍历的时候遇见了`p`或者`q`或者是`nullptr`时返回本身
  - `if(root == NULL || root == q || root == p) return root;`
- 确定本层逻辑
  - 左右子树分别递归