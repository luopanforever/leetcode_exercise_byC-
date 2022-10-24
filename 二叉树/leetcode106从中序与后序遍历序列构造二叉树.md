### [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

**题目描述：**

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 

#### 容易理解的做法

**题解思路：**

由中序遍历序列与后序遍历序列或者与前序遍历序列可以唯一的确定一颗二叉树，本题给出的是中序与后序，则由后序的最后一个结点可以唯一的确定根节点，然后就可以找出中序中的根节点，在中序中，根节点的左右子数组分别代表根节点的左右子树序列，可以使用递归来解

**递归三部曲：**

- 确定参数与返回值

  - 参数需要一个前序子序列和一个后序子序列
  - 返回值返回`TreeNode*`来让**”**主函数**”**接收

- 确定终止条件

  - 首先判断左右子序列是否为空，防止给定的左右子序列过短而直接传进来的数组为0而导致错误，为空的话就直接返回NULL
    - 例如：[2, 1]  \  [2, 1]
  - 然后可以为了缩短时间可以在root创建之后加一个叶子节点的判断，如果左右子序列只有一个值了，也就是访问到叶子节点了，则直接返回root

- 确定本层逻辑

  - 在中序中找到切割点的索引值，然后遵循一个相同的原则来切割数组，我遵循的是左闭右开的原则，故有以下代码

    - ```c++
      int i = 0;
      for(; i < inorder.size(); i++){
        if(inorder[i] == postval) break;
      }
      // 遵循左闭右开
      vector<int> leftleft(inorder.begin(), inorder.begin() + i);
      vector<int> leftright(inorder.begin() + i + 1, inorder.end());  // 加一是为了舍弃中间的根节点元素
      postorder.pop_back(); // 去掉根节点的值
      vector<int> rightleft(postorder.begin(), postorder.begin() + leftleft.size());
      vector<int> rightright(postorder.begin() + leftleft.size(), postorder.end());
      ```
    
  - 然后令左右子树分别等于递归的值
  
    - ```c++
      root->left = constructT(leftleft,rightleft);
      root->right = constructT(leftright,rightright);
      ```

**完整代码**

```c++
class Solution {
public:
    
    TreeNode* constructT(vector<int> & inorder, vector<int> & postorder){
        if(inorder.size() == 0 || postorder.size() == 0) return nullptr;
        int postval = postorder[postorder.size()-1];
        TreeNode *root = new TreeNode(postval);
        
        int i = 0;
        for(; i < inorder.size(); i++){
            if(inorder[i] == postval) break;
        }
        // 遵循左闭右开
        vector<int> leftleft(inorder.begin(), inorder.begin() + i);
        vector<int> leftright(inorder.begin() + i + 1, inorder.end());  // 加一是为了舍弃中间的根节点元素
        postorder.pop_back();
        vector<int> rightleft(postorder.begin(), postorder.begin() + leftleft.size());
        vector<int> rightright(postorder.begin() + leftleft.size(), postorder.end());

        root->left = constructT(leftleft,rightleft);
        root->right = constructT(leftright,rightright);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        TreeNode *result = constructT(inorder, postorder);
        return result;
    }
};
```

#### 优化解法

上述方法空间复杂度太高了，可以加参数来通过索引下标来操作分割

- 参数与返回值
  - 参数多加了四个参数，分别是中序序列的首尾索引(2、3参数)和后序序列的首尾索引(5、6)参数
    - `TreeNode* constructT(vector<int>& inorder,int inorderbegin, int inorderend, vector<int>& postorder, int postorderbegin, int postorderend)`
  - 返回值还是`TreeNode*`便于**“主函数”**接收
- 终止条件
  - 空判断与叶子节点判断
    - 空判断：当任一子序列的收尾索引相等时返回`nullptr`或者是`NULL`
    - 叶子判断：当任一子序列的收尾索引相差1时提前结束本层递归返回root
- 本层逻辑
  - 找切割点与上面的方法相同，都是for循环遍历
  - 不同点在于切割的是索引不是数组，`inorder`与`postorder`数组在整个过程一直不会变，变的是传入的索引，利用此来控制切割的数组

**完整代码**

```c++
class Solution {
public:
    TreeNode* constructT(vector<int>& inorder,int inorderbegin, int inorderend, vector<int>& postorder, int postorderbegin, int postorderend){
        if(inorderbegin == inorderend) return nullptr;

        int postorderval = postorder[postorderend - 1];
        TreeNode* root = new TreeNode(postorderval);

        if(inorderend - inorderbegin == 1) return root;
        // 切割点(cut point)
        int cp;
        for(cp = inorderbegin; cp < inorderend; cp++){
            if(inorder[cp] == postorderval) break;
        }
        // 前序左子序列
        int inorderleftbegin = inorderbegin;
        int inorderleftend = cp;
        // 前序右子序列
        int inorderrightbegin = cp + 1;
        int inorderrightend = inorderend;
        
        // 前序左子序列的长度
        int diff = inorderleftend - inorderleftbegin;

        // 后序左子序列
        int postorderleftbegin = postorderbegin;
        int postorderleftend = postorderbegin + diff;
        // 后续右子序列
        int postorderrightbegin = postorderbegin + diff;
        int postorderrightend = postorderend - 1;

        root->left = constructT(inorder, inorderleftbegin, inorderleftend, postorder, postorderleftbegin, postorderleftend);
        root->right = constructT(inorder, inorderrightbegin, inorderrightend, postorder, postorderrightbegin, postorderrightend);

        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if(inorder.size() == 0 || postorder.size() == 0) return nullptr;
        return constructT(inorder, 0, inorder.size(), postorder, 0, postorder.size());
    }
};
```



​					

