**leetcode590/589[N 叉树的后序遍历](https://leetcode.cn/problems/n-ary-tree-postorder-traversal/)、[N 叉树的前序遍历](https://leetcode.cn/problems/n-ary-tree-preorder-traversal/)**

**递归**

```c++

class Solution {
public:
    void preorder_(Node * root, vector<int> &result){
        if(!root) return;
        result.push_back(root->val);  //这是前序 调整该代码到for循环之后变成后序
        for(int i =0;i < root->children.size(); i++){
            preorder_(root->children[i], result);
        }
    }
    vector<int> preorder(Node* root) {
        vector<int> result;
        preorder_(root, result);
        return result;
    }
};
```

