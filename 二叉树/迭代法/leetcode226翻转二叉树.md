#### leetcode226[翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

#### **迭代法**

- **深度优先遍历：**

  - **不统一的前序遍历：**

    - ```c++
      TreeNode* invertTree(TreeNode* root) {
      stack<TreeNode*> s;
      if(!root) return root;
      else s.push(root);
      while(!s.empty()){
        TreeNode * cur = s.top();
        s.pop();
        swap(cur->left,cur->right);
        if(cur->right)  s.push(cur->right);
        if(cur->left) s.push(cur->left);
      }
      return root;
      }
  
  
    - **统一的前序遍历：**
  
  
      - ```c++
        TreeNode* invertTree(TreeNode* root) {
            stack<TreeNode*> s;  // 动态保存移动的节点
            if(!root) return root;  
            else s.push(root);
            while(!s.empty()){
                TreeNode *cur = s.top();
                if(cur){
                    s.pop();
                    if(cur->right) s.push(cur->right);
                    if(cur->left) s.push(cur->left);
                    s.push(cur);
                    s.push(nullptr);
                }else{
                    s.pop();
                    TreeNode *node = s.top();
                    swap(node->left,node->right);
                    s.pop();
                }
            }
            return root;
        }
        ```
  
- 广度优先遍历

  - 层序遍历

    - ```c++
      TreeNode* invertTree(TreeNode* root) {
              queue<TreeNode*> q;
              if(!root) return root;
              else q.push(root);
              while(!q.empty()){
                  int size = q.size();
                  for(int i = 0; i < size; i++){
                      TreeNode * cur = q.front();
                      q.pop();
                      swap(cur->left,cur->right);
                      if(cur->left) q.push(cur->left);
                      if(cur->right) q.push(cur->right);
                  }
              }
              return root;
          }
      ```

      