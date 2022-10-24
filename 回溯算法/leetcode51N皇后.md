#### [51. N 皇后](https://leetcode.cn/problems/n-queens/)

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

**示例 1：**

![img](image\leetcode51.png)

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：[["Q"]]
```

**提示：**

- `1 <= n <= 9`

**题解思路：**

**设置全局数组**

```c++
vector<vector<string> > result;
vector<string> path; 
```

**构造回溯抽象树**

这个棋盘是一个二维矩阵，对于二维矩阵的回溯，外层for循环用于选择某行中的某一列，内部递归用于选择下一行中的某一列，假设输入样例为`4`，则只看外层`for`循环不管递归部分则有下图

![leetcode51_回溯抽象树_1](image\leetcode51_回溯抽象树_1.png)

递归则是在下一层选择一个节点，下图是一个正确答案的路径

![leetcode51_回溯抽象树_2](image\leetcode51_回溯抽象树_2.png)

**递归三步**

- 确定参数列表和返回值
  - 遍历整颗树不需要返回值
  - 需要输入样例n来为终止条件提供帮助，需要一个`row`变量来控制进入树的下一层
- 确定终止条件
  - 由于是一个`nxn`大小的棋盘，则只要遍历的row达到了n即可进入终止条件

- 确定本层逻辑

  - 根据皇后规则，需要一个函数来判断当前位置是否有效来筛选正确的题解，这就是外层f`or`循环中的筛选条件，筛选规则是该点的十字路径和左`45°`右`45°`路径上都不允许出现`Q`节点，该函数定义如下

    ```c++
    bool isValid(int row, int col, vector<string>& chessboard, int n) {
        // 检查列
        for (int i = 0; i < row; i++) {
            if (chessboard[i][col] == 'Q') {
                return false;
            }
        }
        // 检查 45度角是否有皇后
        for (int i = row - 1, j = col - 1; i >=0 && j >= 0; i--, j--) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        // 检查 135度角是否有皇后
        for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        return true;
    }
    ```

    由于该回溯算法是从`row == 0`的地方开始，且每一行只会选择一个位置，因此不必判断同一行上是否有节点，而且也只用考虑上半区对应位置是否有`Q`即可。row参数在每次递归的时候都 + 1，使其进入下一层的选择，以往的题都是一维的，用startindex来控制后移节点，对应代码如下

    ```c++
    for(int col = 0; col < n; col++){
        	if(isValid(row, col, path, n)){
        	path[row][col] = 'Q';
        	backtracking(n, row + 1);
        	path[row][col] = '.';
        }
    }
    ```

    

**完整代码**

```c++
class Solution {
public:

    bool isValid(int row, int col, vector<string>& chessboard, int n) {
    // 检查列
    for (int i = 0; i < row; i++) { // 这是一个剪枝
        if (chessboard[i][col] == 'Q') {
            return false;
        }
    }
    // 检查 45度角是否有皇后
    for (int i = row - 1, j = col - 1; i >=0 && j >= 0; i--, j--) {
        if (chessboard[i][j] == 'Q') {
            return false;
        }
    }
    // 检查 135度角是否有皇后
    for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (chessboard[i][j] == 'Q') {
            return false;
        }
    }
    return true;
    }
    void backtracking(int n, int row){
        if(row == n){
            result.push_back(path);
            return ;
        }
        for(int col = 0; col < n; col++){
            if(isValid(row, col, path, n)){
                path[row][col] = 'Q';
                backtracking(n, row + 1);
                path[row][col] = '.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        path = vector<string>(n, string(n , '.'));
        backtracking(n, 0);
        return result;
    }
private:
    vector<string> path;
    vector<vector<string>> result;
};
```

​    