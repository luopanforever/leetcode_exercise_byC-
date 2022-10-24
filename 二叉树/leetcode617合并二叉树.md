#### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

**题目描述**

给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** `null` 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

**题解思路**

- 递归

  - 确定参数和返回值。与**“主函数”**一样即可，返回`TreeNode*`用来主函数和递归函数接收

  - 确定终止条件。

    - ```c++
      if(root1 == nullptr) return root2;
      if(root2 == nullptr) return root1;
      ```

    - 经历这两个条件的筛选后，肯定两节点都是存在的

  - 确定本层逻辑

    - 利用前序遍历的思想

      - ```c++
        TreeNode *root = new TreeNode(root1->val + root2->val);
        root->left = bianli(root1->left, root2->left);
        root->right = bianli(root1->right, root2->right);
        ```

  - **完整代码：**

    - ```c++
      class Solution {
      public:
          TreeNode* bianli(TreeNode* root1, TreeNode* root2){
              if(root1 == nullptr) return root2;
              if(root2 == nullptr) return root1;
              TreeNode *root = new TreeNode(root1->val + root2->val);
              root->left = bianli(root1->left, root2->left);
              root->right = bianli(root1->right, root2->right);
      
              return root;
          }
          TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
              return bianli(root1, root2);
          }
      };
      ```

- 迭代

  - 利用层序遍历的思想，利用队列，动态的改变要访问的节点

  - 首先一来也是判断他俩是否是空节点，本迭代的思想是改变左树，而不是创建一个新的树，故都是对树1的操作

  - 首先将左右树的根节点都入队

    - ```c++
      queue<TreeNode*> que;
      que.push(root1);
      que.push(root2);
      ```

  - 接下来就要通过循环来动态访问树中的子节点了

    - `while`循环的条件是队列非空

    - 这两行代码是精髓，他俩会在每次的循环动态访问树中的子节点

      - ```c++
        TreeNode *node1 = que.front();que.pop();
        TreeNode *node2 = que.front();que.pop();
        ```

    - 后面就是一系列的判断条件来让子节点入队，左右数的左右子节点必须成对的入队

      - ```c++
        if(node1->left&&node2->left){
            que.push(node1->left);
            que.push(node2->left);
        }
        if(node1->right&&node2->right){
            que.push(node1->right);
            que.push(node2->right);
        }
        ```

    - **完整代码**

      - ```c++
        class Solution {
        public:
            TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
                if(root1 == nullptr) return root2;
                if(root2 == nullptr) return root1;
                queue<TreeNode*> que;
                que.push(root1);
                que.push(root2);
                
                while(!que.empty()){
                    TreeNode *node1 = que.front();que.pop();
                    TreeNode *node2 = que.front();que.pop();
                    node1->val += node2->val;
                    if(node1->left&&node2->left) {
                        que.push(node1->left);
                        que.push(node2->left);
                    }
                    if(node1->right&&node2->right){
                        que.push(node1->right);
                        que.push(node2->right);
                    }
                    if(!node1->left&&node2->left){
                        node1->left = node2->left;
                    }
                    if(!node1->right&&node2->right){
                        node1->right = node2->right;
                    }
                }
                return root1;
            }
        };
        ```

