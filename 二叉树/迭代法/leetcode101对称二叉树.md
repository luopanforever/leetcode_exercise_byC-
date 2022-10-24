#### leetcode101[对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

**迭代法**

- 层序遍历解决

  - 内层for循环多加了三个else语句判定，用于处理null指针，修改层序遍历，使其数组中不仅仅只放有值的节点，还要放非空节点的左右空孩子进来，这样是为了后续利用双指针来判断是否对称

  - ```c++
    class Solution {
    public:
        bool isSymmetric(TreeNode* root) {
            bool result_ = true;
            queue<TreeNode *> q;
            vector<vector<int>> result;
            if(!root) return !result_;
            else q.push(root);
            while(!q.empty()){
                int size = q.size();
                vector<int> v;
                for(int i = 0; i < size; i++){
                    TreeNode * cur = q.front();
                    q.pop();
                    if(cur) v.push_back(cur->val);
                    else {  
                        v.push_back(-999);
                        continue;  // 当前访问的是空指针  不能继续访问空指针的左右孩子  这是不允许的  所以用continue来解决
                    }
                    if(cur->left) q.push(cur->left);
                    else q.push(nullptr);
                    if(cur->right) q.push(cur->right);
                    else q.push(nullptr);
                }
                result.push_back(v);
            }
            //对result中的每个vector利用双指针进行相同性判定
            for(int i =0;i<result.size();i++){
                for(int j =0,k=result[i].size()-1;j<k;j++,k--){
                    if(result[i][j] != result[i][k]){
                        result_ = false;
                        return result_;
                    }
                }
            }
            return result_;
        }
    };
    ```

    