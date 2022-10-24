#### leetcode100[相同的树](https://leetcode.cn/problems/same-tree/)

递归部分与101相似

```c++
bool compare(TreeNode* left, TreeNode* right){	
	if(!left&&!right) return true;  
    if(!left || !right || left->val != right->val) return false; 
	/*详细写法
    if(left == nullptr && right != nullptr) return false;  //左节点为空右节点不为空
    if(left != nullptr && right == nullptr) return false;  //左节点不为空右节点为空	
    if(left == nullptr && right == nullptr) return true;   //左右节点都为空
    if(left->val != right->val) return false;              //左右节点值不等的情况
    */
	//与101不同的地方就在下面两行代码的参数不同
    bool out = ifSy(left->right,right->left);  //放入左的左   右的左
    bool in = ifSy(left->left,right->right);  //放入左的右   右的右
    return out&&in;
}
```

”主“函数部分直接返回即可

```c++
bool isSameTree(TreeNode* p, TreeNode* q) {
        return compare(p,q);
    }
```

