**题目描述：**

给定一个二叉树的根节点 root ，返回 它的 中序 遍历 。 

**示例 1：**
```c++
输入：root = [1,null,2,3]
输出：[1,3,2]
```
**示例 2：**
```c++
输入：root = []
输出：[]
```
**示例 3：**
```c++
输入：root = [1]
输出：[1]
```

**提示：**

- 树中节点数目在范围 [0, 100] 内
- -100 <= Node.val <= 100

**题解思路：**

二叉树中序遍历的迭代方法，利用指针来访问节点，一直往左节点访问，直到左节点为空则停止，然后是根，右节点

- 利用栈来保存访问的节点

**代码：**

```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> s;
        vector<int> result;
        TreeNode * temp = root; 
        while(temp || !s.empty()){
            if(temp){  // 一直访问存在的左节点
                s.push(temp);  //放入左节点
                temp = temp->left;
            }else{
                temp = s.top();  //相当于null的父节点，此null是父节点的左节点
                s.pop();
                result.push_back(temp->val);  //这一步就相当于访问根节点
                temp = temp->right;
            }
        }
        return result;
    }
};
```

