#### [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

**题目描述**

给你一个含重复值的二叉搜索树（BST）的根节点 `root `，找出并返回 BST 中的所有 [众数](https://baike.baidu.com/item/众数/44796)（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 **任意顺序** 返回。

假定 BST 满足如下定义：

- 结点左子树中所含节点的值 **小于等于** 当前节点的值

- 结点右子树中所含节点的值 **大于等于** 当前节点的值

- 左子树和右子树都是二叉搜索树

**题解思路**

- 迭代法

  - 中序遍历二叉树，将节点放入一个`vector`数组中，三个`for`循环来处理该`vector`

    - 第一个`for`循环将vector放进map中

      - ```c++
        map<int, int> m;
        for(int i = 0; i < v.size(); i++){
            m[v[i]]++;
        }
        ```

    - 第二个`for`循环找到频率最大的key

      - ```c++
        int max = INT_MIN;
        for(map<int, int>::iterator it = m.begin(); it != m.end(); it++){
        	 max = max < it->second ? it->second : max; 
        }
        ```

    - 第三个`for`循环根据最大频率值找到与之匹配的`key`并且放入结果`vector`

      - ```c++
        vector<int> result;
        for(map<int, int>::iterator it = m.begin(); it != m.end(); it++)
            if(it->second == max) result.push_back(it->first);
        ```

  - **完整代码**

    - ```c++
      class Solution {
      public:
          vector<int> findMode(TreeNode* root) {
              stack<TreeNode*> s;
              vector<int> v;
              int diff = 1000000;
              s.push(root);
              while(!s.empty()){
                  TreeNode* cur = s.top(); s.pop();
                  if(cur){
                      if(cur->right) s.push(cur->right);
                      s.push(cur);
                      s.push(nullptr);
                      if(cur->left) s.push(cur->left);
                  }else{
                      cur = s.top(); s.pop();
                      v.push_back(cur->val);
                  }
              }
              map<int, int> m;
              for(int i = 0; i < v.size(); i++){
                  m[v[i]]++;
              }
              int max = INT_MIN;
              
              for(map<int, int>::iterator it = m.begin(); it != m.end(); it++){
                  max = max < it->second ? it->second : max; 
              } 
              vector<int> result;
              for(map<int, int>::iterator it = m.begin(); it != m.end(); it++)
                  if(it->second == max) result.push_back(it->first);
              return result;     
          }
      };
      ```

- 迭代

  - 中序遍历大框架来进行操作

    - ``` c++
      void inorder(TreeNode* root){
      	if(!root) return;
          inorder(root->left);
          // 处理逻辑
          inorder(root->right);
      }
      ```

    - 由于二叉搜索树的中序遍历是升序的，所以可以用记录前一个节点的方法来处理众数，设置一个窗口变量，某数的频率有多高这个窗口变量就有多大，然后设置一个最大窗口变量来代表结果，下列是其具体处理逻辑

      - ```c++
        if(postnode){
            if(postnode->val == root->val)
                chuangkou++;
            else
                chuangkou = 1;
        }else{
            chuangkou = 1;
        }
        postnode = root;
        
        if(chuangkou == maxchuangkou){
            result.push_back(root->val);
        }
                
        if(chuangkou > maxchuangkou){
            maxchuangkou = chuangkou;
            result.clear();
            result.push_back(root->val);
        }
        ```

  - **完整代码**

    - ```c++
      class Solution {
      public:
          int chuangkou;
          int maxchuangkou;
          vector<int> result;
          TreeNode *postnode;
          void inorder(TreeNode* root){
              if(!root) return;
              inorder(root->left);
      
              if(postnode){
                  if(postnode->val == root->val)
                      chuangkou++;
                  else
                      chuangkou = 1;
              }else{
                  chuangkou = 1;
              }
              postnode = root;
      
              if(chuangkou == maxchuangkou){
                  result.push_back(root->val);
              }
              
              if(chuangkou > maxchuangkou){
                  maxchuangkou = chuangkou;
                  result.clear();
                  result.push_back(root->val);
              }
      
              inorder(root->right); 
          }
          vector<int> findMode(TreeNode* root) {
                inorder(root);
                return result;
          }
      };
      ```



