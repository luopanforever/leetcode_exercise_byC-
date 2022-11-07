## [63. 不同路径 II - 力扣（Leetcode）](https://leetcode.cn/problems/unique-paths-ii/description/)

相较于62不同路径1，多加了一个障碍物，不能通过障碍物

**题解思路**

在初始化`dp`数组的时候需要注意，在起始点的同行同列遇见障碍物则跳过

状态转移的时候遍历到障碍直接跳过

**动规五步**

- 确定`dp`数组及下标含义
  -  到达`[i, j]`网格总共由多少条不同路径

- 确定状态转移方程

  - `dp[i][j] = dp[i - 1][j] + dp[i][j - 1];`

    机器人起始与左上角，移动智能向下或者向右，所以[i, j]位置的就可以由左位置右移和上位置下移的和来决定

- 初始化`dp`数组

  - ```c++
    for(int i = 0; i < row && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
    for(int j = 0; j < col && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;
    
    ```

- 确定遍历顺序
  - 自底向下
- 手动推导`dp`数组

**完整代码**

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int row = obstacleGrid.size();
        int col = obstacleGrid[0].size();
        vector<vector<int>> dp(row, vector<int>(col, 0));
        
        for(int i = 0; i < row && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
        for(int j = 0; j < col && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;
        
        for(int i = 1; i < row; i++){
            for(int j = 1; j < col; j++){
                if(obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        
        return dp[row - 1][col - 1];
    }
};
```

