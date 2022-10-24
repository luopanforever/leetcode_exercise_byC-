**递归**

任选一种遍历，在其中加入左叶子节点的判断条件即可

```c++
class Solution {
public:
    //左叶子的判断条件：只有左节点，且左节点的左右孩子节点都为空
    //if(cur->left&&!cur->left->left&&cur->left->right);
    void recursive(TreeNode *root, int &sum){
        if(!root) return;
        if(root->left&&!root->left->left&&!root->left->right) sum+=root->left->val;
        if(root->left) recursive(root->left,sum);

        if(root->right) recursive(root->right,sum);
    }
    int sumOfLeftLeaves(TreeNode* root) {
        int sum =0;
        if(!root) return sum;
        recursive(root,sum);
        return sum;
    }
};
```

**迭代**

利用栈的非统一前序遍历来解

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode *> s;
        vector<int> result;
        if(root == nullptr) return result;
        else s.push(root);
        while(!s.empty()){
            TreeNode *cur =  s.top();
            s.pop();
            result.push_back(cur->val);
            if(cur->right) s.push(cur->right);
            if(cur->left) s.push(cur->left); 
        }
        return result;   
    }
};
```

利用栈的统一前序遍历来解

```c++
class Solution {
public:
    //左叶子的判断条件：只有左节点，且左节点的左右孩子节点都为空
    //if(cur->left&&!cur->left->left&&!cur->left->right);
    int sumOfLeftLeaves(TreeNode* root) {
        stack<TreeNode *> s;
        int sum = 0;
        if(root == nullptr) return sum;
        else s.push(root);
        while(!s.empty()){
            TreeNode *cur =  s.top();
            if(cur){
                s.pop();
                if(cur->right) s.push(cur->right);  //右
                if(cur->left) s.push(cur->left);   //左
                s.push(cur);   //中
                s.push(nullptr);
            }else{
                s.pop();
                cur = s.top();
                s.pop();
                if(cur->left&&!cur->left->left&&!cur->left->right) sum+=cur->left->val;
            }
        }
        return sum;   
    }
};

```



