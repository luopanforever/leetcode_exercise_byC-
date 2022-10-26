#### [37. 解数独](https://leetcode.cn/problems/sudoku-solver/)

编写一个程序，通过填充空格来解决数独问题。

数独的解法需 **遵循如下规则**：

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1：**

![leetcode37_1](E:\csdn_blog博客\leetcode刷题\回溯算法\image\leetcode37_1.png)

```
输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：
```

 

**提示：**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` 是一位数字或者 `'.'`
- 题目数据 **保证** 输入数独仅有一个解

**题解思路**

本题使用的回溯是二维回溯，与以往不同，本题是一个嵌套`for`循环中有个递归，形式类似如下

```c++
for i in range(row)
    for j in range(col)
        // backtracking()...
```

可以拆解一下，首先固定某行，然后固定某列，在一个具体位置选择从`'0'-'9'`的值。具体代码如下

```c++
for(int row = 0; row < 9; row++){
    for(int col = 0; col < 9; col++){
    	// 跳过已经存在的位置
    	for(char number = '1'; number <= '9'; number++){
         	// backtracking 相关
		}
		return false;
	}
}
```

**构造回溯抽象树**

首先只看内层循环，将其看做一维的，其实就是在某行中进行枚举操作，假设此时`row == 0`。画出如下图

![leetcode37回溯抽象树_1](image\leetcode37回溯抽象树_1.png)

这棵树下面还有子节点，庞大且重复。

本题与N皇后不同的是，在同一行会进行多个选择，N皇后则是一行中选择一个即可

**本题的前进动力**

在嵌套循环的首部中，加入忽略已存在数字的节点，这促使了递归的前进，所以不需要`startIndex`。

**在放数字前要检查是否合格**

```c++
for(char number = '1'; number <= '9'; number++){
	if(Valid(board, row, col, number)){
		// backtracking 相关
	}
}
```

关于`valid`函数

```c++
int Valid(vector<vector<char>>& board, int rows, int cols, char number){
        //查看某一行是否有相同的
    for(int col = 0; col < 9; col++){
        if(board[rows][col] == number) return false; 
    }
        // 查看某一列是否有相同的
    for(int row = 0; row < 9; row++){
        if(board[row][cols] == number) return false;
    }
        // 查看某一九宫格内是否有相同的
    int cellrow;
    int cellcol;
    if(rows >=0 && rows < 3) cellrow = 0;
    else if(rows >= 3 && rows < 6) cellrow = 3;
    else cellrow = 6;
    if(cols >=0 && cols < 3) cellcol = 0;
    else if(cols >= 3 && cols < 6) cellcol = 3;
    else cellcol = 6;
    for(int i = cellrow; i < cellrow + 3; i++){
        for(int j = cellcol; j < cellcol + 3; j++){
            if(board[i][j] == number) return false; 
        }
    }
    return true;
}
```

自己在判断九宫格内的索引处理麻烦了，还可以

```c++
int cellrow = (rows / 3) * 3;
int cellcol = (cols / 3) * 3;
```

**递归三步**

- 确定参数和返回类型

  - 本题是确定了某一正确树枝路径就返回，所以需要一个`bool`型的返回值
  - 参数只需要原始数组即可

- 确定终止条件

  - 无需终止条件，因为递归的下一层的棋盘一定比上一层的棋盘多一个数，等数填满了棋盘自然就终止

- 确定本层逻辑

  - ```c++
    bool backtracking(vector<vector<char>>& board){
        for(int row = 0; row < 9; row++){
            for(int col = 0; col < 9; col++){
                if(board[row][col] != '.') continue;  // 递归
                for(char number = '1'; number <= '9'; number++){
                    if(Valid(board, row, col, number)){
                        board[row][col] = number;
                        if(backtracking(board)) return true;
                        board[row][col] = '.';
                    }
                }
                return false;
            }
        }
        return true;
    }
    ```

  - 注意在内层for循环结束后要返回`false`，标志着所有数字在本位置都不合格，所以要返回`false`给调用方传递信息

**完整代码：**

```c++
class Solution {
public:
    int Valid(vector<vector<char>>& board, int rows, int cols, char number){
        //查看某一行是否有相同的
        for(int col = 0; col < 9; col++){
            if(board[rows][col] == number) return false; 
        }
        // 查看某一列是否有相同的
        for(int row = 0; row < 9; row++){
            if(board[row][cols] == number) return false;
        }
        // 查看某一九宫格内是否有相同的
        int cellrow;
        int cellcol;
        if(rows >=0 && rows < 3) cellrow = 0;
        else if(rows >= 3 && rows < 6) cellrow = 3;
        else cellrow = 6;
        if(cols >=0 && cols < 3) cellcol = 0;
        else if(cols >= 3 && cols < 6) cellcol = 3;
        else cellcol = 6;
        for(int i = cellrow; i < cellrow + 3; i++){
            for(int j = cellcol; j < cellcol + 3; j++){
                if(board[i][j] == number) return false; 
            }
        }
        return true;
    }
    bool backtracking(vector<vector<char>>& board){
        for(int row = 0; row < 9; row++){
            for(int col = 0; col < 9; col++){
                if(board[row][col] != '.') continue;  // 递归
                for(char number = '1'; number <= '9'; number++){
                    if(Valid(board, row, col, number)){
                        board[row][col] = number;
                        if(backtracking(board)) return true;
                        board[row][col] = '.';
                    }
                }
                return false;
            }
        }
        return true;
    }
    void solveSudoku(vector<vector<char>>& board) {
        // 0-2 3-6 7-9
        backtracking(board);
    }
};
```

