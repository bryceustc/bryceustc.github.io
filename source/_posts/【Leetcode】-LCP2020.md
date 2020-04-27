---
title: 【Leetcode】 LCP2020
date: 2020-04-25 17:04:22
tags: Leetcode
categories: Leetcode
---
# LCP2020
记录一下第一次参加力扣编程赛LCP，可惜做出来题目不多，还需要不断提升自己，以后参加的周赛也记录一下，好好总结提升代码能力。
<!--more-->
## [LCP 06. 拿硬币](https://leetcode-cn.com/problems/na-ying-bi/)

### 题目
桌上有 `n` 堆力扣币，每堆的数量保存在数组`coins`中。我们每次可以选择任意一堆，拿走其中的一枚或者两枚，求拿完所有力扣币的最少次数。

示例 1：
```
输入：[4,2,1]

输出：4

解释：第一堆力扣币最少需要拿 2 次，第二堆最少需要拿 1 次，第三堆最少需要拿 1 次，总共 4 次即可拿完。
```

示例 2：
```
输入：[2,3,10]

输出：8
```

限制：
- 1 <= n <= 4
- 1 <= coins[i] <= 10

### 思路

1. 直接暴力求解即可。

### 代码

```c++
class Solution {
public:
    int minCount(vector<int>& coins) {
        int res = 0;
        for(auto &c : coins)
        {
            res += (c+1)/2;
        }
        return res;
    }
};
```
## [LCP 07. 传递信息](https://leetcode-cn.com/problems/chuan-di-xin-xi/)

### 题目

小朋友 A 在和ta的小伙伴们玩传信息游戏，游戏规则如下：

1. 有 n 名玩家，所有玩家编号分别为 0 ～ n-1，其中小朋友 A 的编号为 0
2. 每个玩家都有固定的若干个可传信息的其他玩家（也可能没有）。传信息的关系是单向的（比如. A 可以向 B 传信息，但 B 不能向 A 传信息）。
3. 每轮信息必须需要传递给另一个人，且信息可重复经过同一个人

给定总玩家数 n，以及按 [玩家编号,对应可传递玩家编号]关系组成的二维数组 relation。返回信息从小A(编号 0 ) 经过 k轮传递到编号为n-1 的小伙伴处的方案数；若不能到达，返回 0。

示例 1：
```
输入：n = 5, relation = [[0,2],[2,1],[3,4],[2,3],[1,4],[2,0],[0,4]], k = 3

输出：3

解释：信息从小 A 编号 0 处开始，经 3 轮传递，到达编号 4。共有 3 种方案，分别是 0->2->0->4， 0->2->1->4， 0->2->3->4。
```

示例 2：
```
输入：n = 3, relation = [[0,2],[2,1]], k = 2

输出：0

解释：信息不能从小 A 处经过 2 轮传递到编号 2
```

限制：
- 2 <= n <= 10
- 1 <= k <= 5
- 1 <= relation.length <= 90, 且 relation[i].length == 2
- 0 <= relation[i][0],relation[i][1] < n 且 relation[i][0] != relation[i][1]

### 思路

1. DFS

    深度优先搜索，找出所有可能的传递方案。枚举每一轮传递玩家的编号和被传递玩家的编号。若当前是最后一轮且信息位于 k 处，则方案总数加 1

2. 动态规划

    从0开始，经过第一轮可以到达的数字的次数是...

    从0开始，经过第二轮可以到达的数字的次数是...

    从0开始，经过第三轮可以到达的数字...

- 定义状态：
    
    dp[i][j]表示经过 i-1 轮传递给编号 j 的人的方案数。

- 状态转移方程：

    $$
    dp[i][j]=\sum_{v=1}^{n} dp[i-1][v] \text { if }(edge[v][j]==true)
    $$
- 初始化：

    dp[0][0] = 1

- 返回结果：

    dp[k+1][n-1]

### 代码

1.DFS 
```c++
class Solution {
public:
    int numWays(int n, vector<vector<int>>& relation, int k) {
        int res = 0;
        for (int i=0;i<relation.size();i++)
        {
            if (relation[i][0] ==0)  // 起始位置是0
            {
                dfs(relation,relation[i][1],0,k,n,res);
            }
        }
        return res;
    }

    void dfs(vector<vector<int>>& relation, int start, int level, int k, int n, int &res)
    {
        if (level==k-1)
        {
            if (start==n-1)
                res++;
            return;
        }
        for (int i=0;i<relation.size();i++)
        {
            if (relation[i][0]==start)  // 上一层的转移编号，是这一层的开始
            {
                dfs(relation,relation[i][1],level+1,k,n,res);
            }
        }
    }
};
```
2.DP
```c++
class Solution {
public:
    int numWays(int n, vector<vector<int>>& relation, int k) {
        vector<vector<int>> dp(k+1,vector<int>(n,0));
        dp[0][0] = 1;
        // 建立图，统计能传递给编号y玩家的所有玩家编号x
        unordered_map<int,vector<int>> graph;
        for(auto v : relation)
        {
            graph[v[1]].push_back(v[0]);
        }
        for(int i = 1; i <= k; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                for(auto v : graph[j])
                {
                    if(dp[i-1][v] > 0)
                    {
                        dp[i][j] += dp[i-1][v];
                    }
                }
            }
        }
        return dp[k][n-1];
    }
};
```

## [LCP 08. 剧情触发时间](https://leetcode-cn.com/problems/ju-qing-hong-fa-shi-jian/)

### 题目
在战略游戏中，玩家往往需要发展自己的势力来触发各种新的剧情。一个势力的主要属性有三种，分别是文明等级（C），资源储备（R）以及人口数量（H）。在游戏开始时（第 0 天），三种属性的值均为 0。

随着游戏进程的进行，每一天玩家的三种属性都会对应增加，我们用一个二维数组 increase来表示每天的增加情况。这个二维数组的每个元素是一个长度为 3 的一维数组，例如[[1,2,1],[3,4,2]]表示第一天三种属性分别增加 1,2,1 而第二天分别增加 3,4,2。

所有剧情的触发条件也用一个二维数组requirements表示。这个二维数组的每个元素是一个长度为 3 的一维数组，对于某个剧情的触发条件 c[i], r[i], h[i]，如果当前 C >= c[i] 且 R >= r[i]且 H >= h[i]，则剧情会被触发。

根据所给信息，请计算每个剧情的触发时间，并以一个数组返回。如果某个剧情不会被触发，则该剧情对应的触发时间为 -1 。

示例 1：
```
输入： increase = [[2,8,4],[2,5,0],[10,9,8]] requirements = [[2,11,3],[15,10,7],[9,17,12],[8,1,14]]

输出: [2,-1,3,-1]

解释：

初始时，C = 0，R = 0，H = 0

第 1 天，C = 2，R = 8，H = 4

第 2 天，C = 4，R = 13，H = 4，此时触发剧情 0

第 3 天，C = 14，R = 22，H = 12，此时触发剧情 2

剧情 1 和 3 无法触发。
```
示例 2：
```
输入： increase = [[0,4,5],[4,8,8],[8,6,1],[10,10,0]] requirements = [[12,11,16],[20,2,6],[9,2,6],[10,18,3],[8,14,9]]

输出: [-1,4,3,3,3]
```

示例 3：
```
输入： increase = [[1,1,1]] requirements = [[0,0,0]]

输出: [0]
```
限制：
- 1 <= increase.length <= 10000
- 1 <= requirements.length <= 100000
- 0 <= increase[i] <= 10
- 0 <= requirements[i] <= 100000

### 思路

1. 
