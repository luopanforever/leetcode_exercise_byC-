#### [332. 重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)

给你一份航线列表 `tickets` ，其中 `tickets[i] = [fromi, toi]` 表示飞机出发和降落的机场地点。请你对该行程进行重新规划排序。

所有这些机票都属于一个从 `JFK`（肯尼迪国际机场）出发的先生，所以该行程必须从 `JFK` 开始。如果存在多种有效的行程，请你按字典排序返回最小的行程组合。

- 例如，行程 `["JFK", "LGA"]` 与 `["JFK", "LGB"]` 相比就更小，排序更靠前。

假定所有机票至少存在一种合理的行程。且所有的机票 必须都用一次 且 只能用一次。

**示例 1：**

![重新安排行程](image\重新安排行程.png)

```
输入：tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
输出：["JFK","MUC","LHR","SFO","SJC"]
```

**示例 2：**

![img](image\重新安排行程2.png)

```
输入：tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
输出：["JFK","ATL","JFK","SFO","ATL","SFO"]
解释：另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"] ，但是它字典排序更大更靠后。
```

 

**提示：**

- `1 <= tickets.length <= 300`
- `tickets[i].length == 2`
- `fromi.length == 3`
- `toi.length == 3`
- `fromi` 和 `toi` 由大写英文字母组成
- `fromi != toi`

**题解思路：**

**存放结果的数据结构**

`vector<string> result;`

**起始地与目的地的映射关系**使用一个如下的数据结构来保存

```c++
unordered_map<string, map<string, int> > nevigates;
```

- 起始节点一样的情况下，使用`map`对目的地进行排序，这样就自然的对答案进行按字典排序自然的就是最小，并增加一个引用计数，为`0`则表示该票不能再使用。

- **构造对应关系**

  ```c++
  for(vector<string> &s : tickets){
      navigates[s[0]][s[1]]++;
  }
  ```

  `tickets`是一个存放`string`的二维数组，其中每个元素都是一个存放string的数组，所以以这种形式的增强`for`循环来遍历是可行的，引用与否都可`AC`，但是引用可以提升效率，减少了一些拷贝留下的低效。

  循环体中的写法说明

  ```c++
  map<int, int> amap;
  amap[0]++; // 构造了一个key-value对 它是<0, 1>对
  
  // 本题中 s[0]就是起始地 s[1]就是目的地
  navigates[s[0]][s[1]]++; // 执行后构造了一个键值对，它是<s[0], <s[1], 1> >
  ```

**《回溯》**

如下是该题的回溯部分，我将一点一点的解释

```c++
for(pair<const string, int>& navigate : navigates[result[result.size() - 1]]){
    if(navigate.second > 0){
    result.push_back(navigate.first);
    navigate.second--;
    if(backtracking(ticketsNum, index + 1)) return true;
    navigate.second++;
    result.pop_back();
	}
}
```

- 增强`for`循环的条件体

  - `pair<const string, int>& navigate : navigates[result[result.size() - 1]]`
    - `result[result.size() - 1]`：该表达式的结果是`result`的最后一个`string`，效果等同于`result.back()`
  - 利用`pair<string, int>& navigate`来接收`navigates`的`value`，此后`navigate.front`就是目的地，`navigate.second`就是引用次数

- 循环体

  - 首先会判断引用次数是否大于`0`，大于`0`就正常进入回溯部分，否则不执行，这避免了重复对一张票子引用

  - 对进入回溯函数之前想`result`中加入当前票的目的地，引用次数减`1`，相当于标记了使用一次该票
  - 在参数列表中有一个`index`的参数，记录`result`数组中的元素个数，在终止条件中会有用的，每次进入回溯会对`index + 1`，记录`result`中的个数

**构造回溯抽象树**

**前置信息**：题目给出，本题的最开始起始地总是`[JKF]`，所以直接在`result`数组中`push`一个`"JKF"`即可

根据以上条件可以判断出树的根节点以`"JKF"`为起始地点的票子，进入回溯就是以`"JKF"`为起始地选择目的地，由于使用的map来保存目的地，所以会以字典序从小到大自动排序，外层`for`循环的遍历顺序也就是以字典序大小来遍历，我以以下输入样例为例来构造回溯抽象树

```c++
[["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
```

对应的图为：

![回溯抽象树332](image\回溯抽象树332.png)

画出回溯抽象树的部分

![回溯抽象树332_1](image\回溯抽象树332_1.png)

题解路径寻找的抽象图

![回溯抽象树332_2](image\回溯抽象树332_2.png)

**递归三步**

- 确定参数列表和返回值

  - 本题不用将整个树遍历完，只要找到了符合的一条树枝，直接返回结果即可，虽然一般的回溯算法不需要返回值，但该题需要一个`bool`返回值来遇见某一符合条件的树枝就返回，由于`map`自动排序，所以返回的树枝一定是字典序最小的
  - 需要一个`index`参数和票的大小参数`ticketsNum`，都是用于终止条件的

- 确定终止条件

  - 根据题目的意思，需要把每一张票都用完，所以`result`数组的大小最后一定是票子的数量`+1`，所以终止条件就是

    `index == ticketsNum + 1`，无需有判断答案是最小字典序，根据前面的说法，`map`已经自动排序，所以第一个返回的题解一定是最小字典序的

- 确定本层逻辑

  - 根据题意：`result`数组的第一个元素一定是`"JKF"`，从`JKF`开始回溯，选择某一目的地后将其加入`result`数组，然后将该目的地作为新的起始地，接着回溯，具体细节看上面的构造抽象回溯树

**完整代码:**

```c++
class Solution {
public:
    // 保存航班信息的数据结构
    unordered_map<string, map<string, int>> navigates;
    // 第二个泛型是map，可以自动排序字典序
    vector<string> result;
    bool backtracking(int ticketsNum, int index){
        if(index == ticketsNum + 1){
            return true;
        }
        for(pair<const string, int>& navigate : navigates[result[result.size() - 1]]){  // result.size() - 1 随着result.size()的增加，其则顺其自然的遍历了后续门票起始
            if(navigate.second > 0){
                result.push_back(navigate.first);
                navigate.second--;
                if(backtracking(ticketsNum, index + 1)) return true;
                navigate.second++;
                result.pop_back();
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        // 起始地与目的地的映射关系
        for(vector<string> s : tickets){
            navigates[s[0]][s[1]]++;
        }
        result.push_back("JFK");
        backtracking(tickets.size(), 1);
        return result;
    }
};
```

