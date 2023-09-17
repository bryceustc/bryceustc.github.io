---
title: 【Leetcode】第207场周赛
date: 2020-09-20 14:38:37
tags: LeetCode
categories: LeetCode
mathjax: true
---

### [5519. 重新排列单词间的空格](https://leetcode-cn.com/problems/rearrange-spaces-between-words/)

给你一个字符串 text ，该字符串由若干被空格包围的单词组成。每个单词由一个或者多个小写英文字母组成，并且两个单词之间至少存在一个空格。题目测试用例保证 text 至少包含一个单词 。

请你重新排列空格，使每对相邻单词之间的空格数目都 相等 ，并尽可能 最大化 该数目。如果不能重新平均分配所有空格，请 将多余的空格放置在字符串末尾 ，这也意味着返回的字符串应当与原 text 字符串的长度相等。

返回 重新排列空格后的字符串 。

<!--more-->
示例1：
```
输入：text = "  this   is  a sentence "
输出："this   is   a   sentence"
解释：总共有 9 个空格和 4 个单词。可以将 9 个空格平均分配到相邻单词之间，相邻单词间空格数为：9 / (4-1) = 3 个。
```

示例2:
```
输入：text = " practice   makes   perfect"
输出："practice   makes   perfect "
解释：总共有 7 个空格和 3 个单词。7 / (3-1) = 3 个空格加上 1 个多余的空格。多余的空格需要放在字符串的末尾。
```

示例3：
```
输入：text = "hello   world"
输出："hello   world"
```

示例4：
```
输入：text = "  walks  udp package   into  bar a"
输出："walks  udp  package  into  bar  a "
```

示例5：
```
输入：text = "a"
输出："a"
```

提示：
- 1 <= text.length <= 100
- text 由小写英文字母和 ' ' 组成
- text 中至少包含一个单词
### 思路
小学除法，直接暴力模拟。

### 代码
```c++
class Solution {
public:
    string reorderSpaces(string text) {
        int n = text.size();
        vector<string> words;
        int cnt = 0;
        string tmp;
        for (int i = 0; i < n; i++) {
            if (text[i] == ' ') {
                cnt++;
                if (tmp.size() > 0) {
                    words.push_back(tmp);
                    tmp = "";
                }
            } else {
                tmp += text[i];
            }
        }
        if (tmp.size() > 0) words.push_back(tmp);
        int m = words.size(), p, q;
        if (m == 1) {
            p =  0;
            q = cnt;
        } else {
            p = cnt / (m - 1);
            q = cnt % (m - 1);
        }
        string res;
        for (int i = 0; i < m; i++) {
            if (i > 0) {
                for (int j = 0; j < p; j++) res += " ";
            }
            res += words[i];
        }
        for (int i = 0; i < q; i++) {
            res += " ";
        }
        return res;
    }
};
```

### [5520. 拆分字符串使唯一子字符串的数目最大](https://leetcode-cn.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)

给你一个字符串 s ，请你拆分该字符串，并返回拆分后唯一子字符串的最大数目。

字符串 s 拆分后可以得到若干 非空子字符串 ，这些子字符串连接后应当能够还原为原字符串。但是拆分出来的每个子字符串都必须是 唯一的 。

注意：子字符串 是字符串中的一个连续字符序列。

示例 1：

```
输入：s = "ababccc"
输出：5
解释：一种最大拆分方法为 ['a', 'b', 'ab', 'c', 'cc'] 。像 ['a', 'b', 'a', 'b', 'c', 'cc'] 这样拆分不满足题目要求，因为其中的 'a' 和 'b' 都出现了不止一次。
```

示例 2：
```
输入：s = "aba"
输出：2
解释：一种最大拆分方法为 ['a', 'ba'] 。
```

示例 3:
```
输入：s = "aa"
输出：1
解释：无法进一步拆分字符串。
```

提示：
- 1 <= s.length <= 16
- s 仅包含小写英文字母

### 思路
注意数据范围，可以利用回溯来做。

- 回溯
- 使用一个哈希验证是否已有重复子串
- 判断剩余字符长度和已有答案，进行剪枝

### 代码
```c++
class Solution {
public:
    int maxUniqueSplit(string s) {
        int n = s.size();
        dfs(s, n, 0);
        return res;
    }
    void dfs(const string& s, int n, int start) {
        if (n - start + st.size() < res) return; // 剪枝
        if (start == n) {
            res = max(res, (int)st.size());
            return;
        }
        string tmp;
        for (int i = start; i < n; i++) {
            tmp += s[i];
            if (st.find(tmp) == st.end()) {
                st.insert(tmp);
                dfs(s, n, i+1);
                st.erase(tmp);
            }
        }
    }
private:
    int res = 0;
    unordered_set<string> st;
};
```

### [5521. 矩阵的最大非负积](https://leetcode-cn.com/problems/maximum-non-negative-product-in-a-matrix/)

给你一个大小为 rows x cols 的矩阵 grid 。最初，你位于左上角 (0, 0) ，每一步，你可以在矩阵中 向右 或 向下 移动。

在从左上角 (0, 0) 开始到右下角 (rows - 1, cols - 1) 结束的所有路径中，找出具有 最大非负积 的路径。路径的积是沿路径访问的单元格中所有整数的乘积。

返回 最大非负积 对 109 + 7 取余 的结果。如果最大积为负数，则返回 -1 。

注意，取余是在得到最大积之后执行的。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/16/q3v1.jpg)
```
输入：grid = [[-1,-2,-3],
             [-2,-3,-3],
             [-3,-3,-2]]
输出：-1
解释：从 (0, 0) 到 (2, 2) 的路径中无法得到非负积，所以返回 -1
```

示例 2：
```
输入：grid = [[1,-2,1],
             [1,-2,1],
             [3,-4,1]]
输出：8
解释：最大非负积对应的路径已经用粗体标出 (1 * 1 * -2 * -4 * 1 = 8)
```

示例 3：
```
输入：grid = [[1, 3],
             [0,-4]]
输出：0
解释：最大非负积对应的路径已经用粗体标出 (1 * 0 * -4 = 0)
```

示例 4:
```
输入：grid = [[ 1, 4,4,0],
             [-2, 0,0,1],
             [ 1,-1,1,1]]
输出：2
解释：最大非负积对应的路径已经用粗体标出 (1 * -2 * 1 * -1 * 1 * 1 = 2)
```

提示：
- 1 <= rows, cols <= 15
- -4 <= grid[i][j] <= 4

### 思路

考虑dp，这种一般都要维护两个值最大值和最小值，这是lc152最大乘积子数组的二维矩阵版本

### 代码

```c++
class Solution {
public:
    int maxProductPath(vector<vector<int>>& grid) {
        const int MOD = 1e9 + 7;
        int m = grid.size();
        int n = grid[0].size();
        // dp[i][j][0]表示从grid[0][0]到grid[i][j]的最小值
        // dp[i][j][1]表示从grid[0][0]到grid[i][j]的最大值
        vector<vector<vector<long long>>> dp(m, vector<vector<long long>> (n, vector<long long>(2,0ll)));
        dp[0][0][0] = grid[0][0];
        dp[0][0][1] = grid[0][0];
        // 初始化第一列
        for (int i = 1; i < m; i++) {
            if (grid[i][0] >= 0) {
                dp[i][0][0] = dp[i-1][0][0] * grid[i][0];
                dp[i][0][1] = dp[i-1][0][1] * grid[i][0];
            } else {
                dp[i][0][0] = dp[i-1][0][1] * grid[i][0];
                dp[i][0][1] = dp[i-1][0][0] * grid[i][0];
            }
            
        }
        // 初始化第一行
        for (int j = 1; j < n; j++) {
            if (grid[0][j] >= 0) {
                dp[0][j][0] = dp[0][j-1][0] * grid[0][j];
                dp[0][j][1] = dp[0][j-1][1] * grid[0][j];
            } else {
                dp[0][j][0] = dp[0][j-1][1] * grid[0][j];
                dp[0][j][1] = dp[0][j-1][0] * grid[0][j];
            }
            
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (grid[i][j] >= 0) {
                    dp[i][j][0] = min(dp[i-1][j][0] * grid[i][j], dp[i][j-1][0] * grid[i][j]);
                    dp[i][j][1] = max(dp[i-1][j][1] * grid[i][j], dp[i][j-1][1] * grid[i][j]);
                } else {
                    dp[i][j][0] = min(dp[i-1][j][1] * grid[i][j], dp[i][j-1][1] * grid[i][j]);
                    dp[i][j][1] = max(dp[i-1][j][0] * grid[i][j], dp[i][j-1][0] * grid[i][j]);
                }
            }
        }
        if (dp[m-1][n-1][1] < 0) return -1;
        return dp[m-1][n-1][1] % MOD;
    }
};
```

### [5522. 连通两组点的最小成本](https://leetcode-cn.com/problems/minimum-cost-to-connect-two-groups-of-points/)

给你两组点，其中第一组中有 size1 个点，第二组中有 size2 个点，且 size1 >= size2 。

任意两点间的连接成本 cost 由大小为 size1 x size2 矩阵给出，其中 cost[i][j] 是第一组中的点 i 和第二组中的点 j 的连接成本。如果两个组中的每个点都与另一组中的一个或多个点连接，则称这两组点是连通的。换言之，第一组中的每个点必须至少与第二组中的一个点连接，且第二组中的每个点必须至少与第一组中的一个点连接。

返回连通两组点所需的最小成本。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/20/ex1.jpg)
```
输入：cost = [[15, 96], [36, 2]]
输出：17
解释：连通两组点的最佳方法是：
1--A
2--B
总成本为 17 。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/20/ex2.jpg)
```
输入：cost = [[1, 3, 5], [4, 1, 1], [1, 5, 3]]
输出：4
解释：连通两组点的最佳方法是：
1--A
2--B
2--C
3--A
最小成本为 4 。
请注意，虽然有多个点连接到第一组中的点 2 和第二组中的点 A ，但由于题目并不限制连接点的数目，所以只需要关心最低总成本。
```

示例3：
```
输入：cost = [[2, 5, 1], [3, 4, 7], [8, 1, 2], [6, 2, 4], [3, 8, 8]]
输出：10
```

提示：
- size1 == cost.length
- size2 == cost[i].length
- 1 <= size1, size2 <= 12
- size1 >= size2
- 0 <= cost[i][j] <= 100

### 思路

因为已知第二组点的数量较少，所以对第二组点的连通状态进行状态压缩，然后依次处理第一组中的点即可。

对于第一组中的每个点，第一种做法是直接连一条边，第二种做法是连接若干个第二组中当前还没有连通的点。对于第一种做法，直接枚举MMM个点；对于第二种做法，假设当前未连通的点为maskmaskmask，我们需要枚举它的子集，这里可以用位运算枚举子集的方法来进行优化。

最后的总时间复杂度为O(NM\cdot3^M)


### 代码

```c++
class Solution {
public:
    int connectTwoGroups(vector<vector<int>> &cost) {
        int size1 = cost.size(), size2 = cost[0].size(), stateNum = 1 << size2;    //stateNum为第二组总的状态数+1
        vector<int> dp(stateNum, INT_MAX);                                         //dp数组初始化为很大的数
        dp[0] = 0;                                                                 //初始状态
        for (int i = 0; i < size1; ++i) {                                          //迭代每一行
            vector<int> temp(stateNum, INT_MAX);                                   //滚动数组
            for (int state = 0; state < stateNum; ++state) {                       //枚举所有状态
                if (dp[state] == INT_MAX) continue;                                //若状态不可达，continue
                for (int j = 0; j < size2; ++j) {                                  //方案一：任选一条边相连
                    int nextState = state | (1 << j);                              //相连后到达的状态
                    temp[nextState] = min(temp[nextState], dp[state] + cost[i][j]);//更新最小花费
                }
                int flipState = (stateNum - 1) ^ state;                                          //方案二：连接若干未连接的边，使用异或进行位反转得到所有未连接的边
                for (int subState = flipState; subState; subState = flipState & (subState - 1)) {//枚举未连接的边的子集
                    int sum = 0;                                                                 //记录花费
                    for (int k = 0; k < size2; ++k)                                              //枚举size2
                        if (subState & (1 << k)) sum += cost[i][k];                              //若子集中存在该边，则更新花费
                    int nextState = state | subState;                                            //相连后到达的状态
                    temp[nextState] = min(temp[nextState], dp[state] + sum);                     //更新最小花费
                }
            }
            dp = move(temp);//滚动数组
        }
        return dp.back();//返回结果
    }
};
```