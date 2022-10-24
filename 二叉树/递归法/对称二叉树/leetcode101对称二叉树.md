#### leetcode101[对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

- **确定递归函数的参数和返回值：**

- **确定终止条件：**
- **确定单层递归的逻辑**

**tips:**写递归的时候只需要关注该层就可以了，下一层的思想直接甩给递归就好，只想前两层就好，第三层拿去递归想

```c++
bool ifSy(TreeNode* left,TreeNode*right){
    if(!left&&!right) return true;  
    if(!left || !right || left->val != right->val) return false; 
	/*详细写法
    if(left == nullptr && right != nullptr) return false;  //左节点为空右节点不为空
    if(left != nullptr && right == nullptr) return false;  //左节点不为空右节点为空	
    if(left == nullptr && right == nullptr) return true;   //左右节点都为空
    if(left->val != right->val) return false;              //左右节点值不等的情况
    */
    bool out = ifSy(left->right,right->left);  //放入左的右   右的左
    bool in = ifSy(left->left,right->right);  //放入左的左   右的右
    return out&&in;
}
bool isSymmetric(TreeNode* root) {
    if(!root) return true;
    return ifSy(root->left,root->right);
}
```

