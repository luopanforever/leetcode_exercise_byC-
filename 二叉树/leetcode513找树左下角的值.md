**迭代**

层序遍历后 直接返回最后一层的0号index节点

```c++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> q;
        vector<vector<int>> result;
        q.push(root);
        while(!q.empty()){
            int size = q.size();
            vector<int> v;
            for(int i = 0;i < size; i++){
                TreeNode *cur = q.front();
                q.pop();
                v.push_back(cur->val);
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right);
            }
            result.push_back(v);
        }
        return result[result.size() - 1][0];
    }
};
```

**递归**

回溯还不懂