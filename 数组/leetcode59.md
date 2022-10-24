#### leetcode59 螺旋矩阵二解题思路
**题目描述**

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

 

**提示：**

- `1 <= n <= 20`

**解题思路：**

可以将其看做是n阶方阵，利用四个for循环进行上下左右的遍历且每次遍历需要按照相同规则

例如向右遍历的时候不遍历整行，将最后一个元素作为向下遍历的开始元素

根据这规则我写出如下代码：

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
	    vector<vector<int>> result(n, vector<int>(n, 0));
	    int loop = n / 2; //完整遍历的圈数
	    int i, j; //横纵坐标 只是符号
	    int num = 1; //将存入容器中的数据
	    if (n % 2 != 0)  result[loop][loop] = n * n;  //判断是否矩阵有中间数  如果有则直接填入
	    for (int cur_loop = 0; cur_loop < loop; cur_loop++) {  //cur_loop代表当前处于的圈数
		    for (j = cur_loop; j < n - cur_loop - 1; j++) {  //向右  每圈存入时横坐标保持不变  纵坐标持续增加
			    result[cur_loop][j] = num++;
		    }
		    for (i = cur_loop; i < n - cur_loop - 1; i++) {  //向下  每圈存入时纵坐标保持不变  横坐标持续增加
			    result[i][n - cur_loop - 1] = num++;
		    }
		    for (j = n - cur_loop - 1; j > cur_loop; j--) {  //向左  每圈存入时横坐标保持不变  纵坐标持续递减
			    result[n - cur_loop - 1][j] = num++;
		    }
		    for (i = n - cur_loop - 1; i > cur_loop; i--) {  //向上  每圈存入时纵坐标保持不变  横坐标持续递减
			    result[i][cur_loop] = num++;
		    }
	    }
	    return result;
    }     
};
```

