#### leetcode110[平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

**迭代法**

先写一个**后序遍历**求二叉树高度的函数  或者**层序遍历**求高度也可

**后序遍历**

```c++
int deepth(TreeNode * root){
        stack<TreeNode*> s;
        int deepth = 0;
        int result = 0;
        if(!root) return 0;
        else s.push(root);
        while(!s.empty()){
            TreeNode * cur = s.top();
            if(cur){
                s.pop();
                s.push(cur);
                s.push(nullptr);
                deepth++;
                if(cur->right) s.push(cur->right);
                if(cur->left) s.push(cur->left);
            }else{
                s.pop();
                cur = s.top();
                deepth--;
                s.pop();
            }
            result = result>deepth?result:deepth;
        }
        return result;
    }
```

**层序遍历**

```c++
int deepth(TreeNode * root){
        queue<TreeNode*> q;
        int deepth = 0;
        if(!root) return deepth;
        else q.push(root);
        while(!q.empty()){
            int size = q.size();
            for(int i =0;i < size; i++){
                TreeNode * cur = q.front();
                q.pop();
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right); 
            }
            deepth++;
        }
        return deepth;
    }
```



然后再用后序遍历来判断二叉树是否是平衡二叉树

```c++
bool isBalanced(TreeNode* root) {
        stack<TreeNode*> s;
        if(!root) return 1;
        else s.push(root);
        while(!s.empty()){
            TreeNode * cur = s.top();
            if(cur){
                s.pop();
                s.push(cur);
                s.push(nullptr);
                if(cur->right) s.push(cur->right);
                if(cur->left) s.push(cur->left);
            }else{
                s.pop();
                cur = s.top();
                int left = deepth(cur->left);
                int right = deepth(cur->right);
                if(abs(left - right) > 1) return false;
                s.pop();
            }
        }
        return true;
    }
```



