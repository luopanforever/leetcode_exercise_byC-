编写一个算法来判断一个数 `n` 是不是快乐数。

**「快乐数」** 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
- 如果这个过程 **结果为** 1，那么这个数就是快乐数。

如果 `n` 是 *快乐数* 就返回 `true` ；不是，则返回 `false` 。

 

**示例 1：**

```
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**示例 2：**

```
输入：n = 2
输出：false
```

**解题思路：**

题目关键词无线重复，意思就是如果该数是一个无线重复的数，则该数最终会陷入一个循环，我们建立一个hash表，每次各数平方和后先判断该数是否在该hash表中，如果不存在则将该数insert进hash表，如果存在则说明重复，则返回false，可以设置一个条件为1的while循环，在每次循环时判断该数是否为1，如果是则返回true；

**代码：**

```c++
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> bo_repetion;
        int sum = 0;
        while(1){
            if(sum ==1) return true;
            sum = 0;
            while(n){
            sum += (n%10)*(n%10);
            n/=10;
            }
  
            if(bo_repetion.find(sum)==bo_repetion.end()){
                bo_repetion.insert(sum);
             }else{
                return false;
            }
            n = sum;
        }
    }
};
```

