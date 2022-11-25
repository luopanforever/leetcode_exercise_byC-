## [337. 打家劫舍 III - 力扣（Leetcode）](https://leetcode.cn/problems/house-robber-iii/description/)

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

**示例 2:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

```
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

**提示：**

- 树的节点数在 `[1, 104]` 范围内
- `0 <= Node.val <= 104`

## **题解思路：**

**暴力解法：**

本题必须用后序遍历，需要用到函数的返回值来做下一步计算

抢了该节点则不能抢该节点的孩子节点

没有抢该节点则考虑抢左右节点

**注意**：是考虑抢左右节点

使用带备忘录的递归，用`map<TreeNode* , int>`来保存

```c++
class Solution {
public:
    unordered_map<TreeNode*, int> meme;  // 备忘录  该备忘录的含义是记录以某节点为根节点可以偷的最大金钱
    int rob(TreeNode* root) {
        if(!root) return 0;  // 终止条件
        if(!root->left && !root->right) return root->val;  // 如果该节点没有左右孩子，则不需要动态规划，取该点的值就是最大的
        
        if(meme[root]) return meme[root];  // 如备忘录中有则直接返回
        // val1表示为取该点
        int val1 = root->val;
        if(root->left) val1 += rob(root->left->left) + rob(root->left->right);
        if(root->right) val1 += rob(root->right->left) + rob(root->right->right);
		
        // val2表示为不取该点
        int val2 = rob(root->left) + rob(root->right);
        meme[root] = max(val1, val2);  // 将计算过的结果放入备忘录中
        return max(val2, val1);  // 返回最大的  这一步就代表了考虑
    }
};
```

**动态规划：**

动态规划其实就是使用状态转移容器来记录状态的变化，这里使用一个长度为2的数组，记录当前结点偷与不投所得到的的最大金钱

这道题是在树上进行状态转移，涉及到递归，则可以结合递归三步与动归五步

- 确定递归函数的参数和返回值/确定`dp`数组及含义
  - 需要记录的是某一节点偷与不偷的两个状态所到的金钱，所以返回值就是一个长度为`2`的数组
  - 参数就是当前结点
  - `dp`数组的含义：大小为`2`，`dp[0]为`不取该点的钱的最高金额，`dp[1]`为取该点的钱的最大金额
  - 此处返回值就是`dp`数组，系统栈会保存每一层递归的参数，所以不用担心长度为`2`的数组不够标记树中每个节点的状态

- 确定终止条件/`dp`数组的初始化
  - 如果遇见空节点，很明显，偷与不偷都是`0`元，所以直接返回`{0， 0}`即可。
  - 这也相当于`dp`数组的初始化，后续遍历是深度优先，会直接遍历到左子树的`NULL`节点，此时是递归最深层，则返回的`dp`数组就随着动态规划而变

- 确定遍历顺序

  - 明确的，使用后续遍历，因为通过递归函数的返回值来做下一步计算。

  - 通过递归左节点，得到左节点偷与不偷的金钱。

  - 通过递归右节点，得到左节点偷与不偷的金钱

    ```c++
    vector<int> left = robTree(cur->left); // 左
    vector<int> right = robTree(cur->right); // 右
    ```

- 确定本层逻辑

  - 偷当前结点

    ```c++
    // 当前节点的值+加上不偷左右子节点的值 （防止触动警报）
    int val_1 = root->val + left[0] + right[0];
    ```

  - 不偷当前结点

    ```c++
    // 如果不偷当前结点，考虑是否偷子节点，因为不偷子节点可能会比偷子节点大，时刻注意dp数组的含义!
    int val_0 = max(left[0], left[1]) + max(right[0], right[0]);  // 核心！
    ```

  - 最后就返回`{val_0, val_1}`

**完整代码:**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // dp数组的含义：大小为2，dp[0]为不取该点的钱的最高金额，dp[1]为取该点的钱的最高金额
    int rob(TreeNode* root) {
        vector<int> result = robChild(root);
        return max(result[0], result[1]);
    }
    vector<int> robChild(TreeNode* root){
        if(!root) return {0, 0};  // 这里相当于dp数组的初始化
        vector<int> left = robChild(root->left);
        vector<int> right = robChild(root->right);

        int val_1 = root->val + left[0] + right[0];
        int val_0 = max(left[1], left[0]) + max(right[1], right[0]);

        return {val_0, val_1};
    }
};
```

