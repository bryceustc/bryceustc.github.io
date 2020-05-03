---
title: 【LeetCode】 LCP2020
date: 2020-04-25 17:04:22
tags: LeetCode
categories: LeetCode
---
# LCP2020
记录一下第一次参加力扣编程赛LCP，可惜做出来题目不多，还需要不断提升自己，以后参加的周赛也记录一下，好好总结提升代码能力。

## [LCP 06. 拿硬币](https://leetcode-cn.com/problems/na-ying-bi/)

### 题目
桌上有 `n` 堆力扣币，每堆的数量保存在数组`coins`中。我们每次可以选择任意一堆，拿走其中的一枚或者两枚，求拿完所有力扣币的最少次数。
<!--more-->
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

### 题意概述
已知编号 0 ~ n-1 玩家的所有单向信息传递路径，初始信息位于编号 0，求信息经过 k 条路径之后到达编号 n-1 玩家的方案总数

### 思路

1. DFS

    深度优先搜索，找出所有可能的传递方案。枚举每一轮传递玩家的编号和被传递玩家的编号。若当前是最后一轮且信息位于 k 处，则方案总数加 1

2. 动态规划

    从0开始，经过第一轮可以到达的数字的次数是...

    从0开始，经过第二轮可以到达的数字的次数是...

    从0开始，经过第三轮可以到达的数字...

- 定义状态：
    
    dp[i][j]表示经过 i轮传递给编号 j 的人的方案数。

- 状态转移方程：

    $$
    dp[i][j]=\sum_{v=1}^{n} dp[i-1][v] \text { if }(edge[v][j]==true)
    $$
- 初始化：

    dp[0][0] = 1

- 返回结果：

    dp[k][n-1]

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
        // dp[i][j]表示经过 i轮传递给编号 j 的人的方案数。
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
                        dp[i][j] += dp[i-1][v];  // 状态转移方程
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


### 题意概述
题目大意是有一个游戏，随着时间的增加我们的属性会不断提升。当属性大于某个临界点时会触发相应剧情，最后问每个剧情触发的时间。

### 思路

1. 当时直接暴力模拟，超时了

2. 利用二分查找来求解

    - 维护一个递增的Increse数组，其每个元素都是严格递增的，对requirements里的每个元素 在Increse数组里二分查找到第一个比它大的，则为首先满足条件的天数。
3. 二分查找的应用，要学会变通

### 代码
```c++
class Solution {
public:
    vector<int> getTriggerTime(vector<vector<int>>& increase, vector<vector<int>>& requirements) {
        vector<int> res;
        int n = increase.size();
        vector<vector<int>> s (n+1, vector<int>(3,0));
        for (int i=0;i<n;++i)
        {
            for (int j=0;j<3;++j)
            {
                s[i+1][j] = s[i][j] + increase[i][j];
            }
        }
        for (auto& v : requirements)
        {
            int start = 0;
            int end = n;
            while(end>start)
            {
                int mid = start + (end-start)/2;
                if (s[mid][0]>=v[0]&&s[mid][1]>=v[1]&&s[mid][2]>=v[2])
                {
                    end = mid;
                }
                else
                {
                    start = mid+1;
                }
            }
            if (s[start][0]>=v[0]&&s[start][1]>=v[1]&&s[start][2]>=v[2])
            {
                res.push_back(start);
            }
            else
            {
                res.push_back(-1);
            }
        }
        return res;
    }
};
```

##  [LCP 09. 最小跳跃次数](https://leetcode-cn.com/problems/zui-xiao-tiao-yue-ci-shu/)

### 题目
为了给刷题的同学一些奖励，力扣团队引入了一个弹簧游戏机。游戏机由 N 个特殊弹簧排成一排，编号为 0 到 N-1。初始有一个小球在编号 0 的弹簧处。若小球在编号为 i 的弹簧处，通过按动弹簧，可以选择把小球向右弹射jump[i]的距离，或者向左弹射到任意左侧弹簧的位置。也就是说，在编号为 i 弹簧处按动弹簧，小球可以弹向 0 到 i-1 中任意弹簧或者 i+jump[i] 的弹簧（若 i+jump[i]>=N ，则表示小球弹出了机器）。小球位于编号 0 处的弹簧时不能再向左弹。

为了获得奖励，你需要将小球弹出机器。请求出最少需要按动多少次弹簧，可以将小球从编号 0 弹簧弹出整个机器，即向右越过编号 N-1 的弹簧。

示例 ：
```
输入：jump = [2, 5, 1, 1, 1, 1]

输出：3

解释：小 Z 最少需要按动 3 次弹簧，小球依次到达的顺序为 0 -> 2 -> 1 -> 6，最终小球弹出了机器。

```

限制：
- 1 <= jump.length <= 10^6
- 1 <= jump[i] <= 10000

### 题意概述：
给定一个数组 jump，长度为 N，在第 i 个位置可以选择跳到 0..i-1 和 i + jump[i]，问从 0 跳过 n-1 的最小跳跃次数是多少。

### 思路

1.dp

此题从后向前进行动态规划计算。

- 定义状态：dp[i]表示第i个弹簧到弹出所需的最小次数
- 状态转移方程：

$$
i< j <n && j< i+ jump[i]

dp[j] = min(dp[j],dp[i]+1); 
$$
- 初始化：
dp[n-1] = 1
-返回值：dp[0]
- 压缩剪枝：dp[j]>=dp[i]+1时，dp = dp[i]+1


2.bfs

求最少跳跃次数

假设jump数组的长度为6
当走到下标为3的位置，我们需要访问下标为0,1,2的位置
当走到下边为4的位置，我们需要访问下标为0,1,2,3的位置
0,1,2这些位置会在之后的位置被不断地访问，这就是bfs算法超时的原因
时间复杂度大概是O(n*n)

- 每次往前跳的时候或者往后跳的时候如果发现部分元素已经访问过时，需要将其剔除掉。因为之后再访问这个位置的步数肯定大于之前访问的步数
- 记录当前步数下所能够跳的最远距离，这时下一布如果选择往回跳时，直接从上一次的最大值开始常识，而不必要从0开始跳起。算法的时间复杂度为O(n).


### 代码

1.dp(易理解，但超时) 
```c++
class Solution {
public:
    int minJump(vector<int>& jump) {
        int n = jump.size();
        // 定义状态：dp[i]表示第i个弹簧到弹出所需的最小次数
        vector<int> dp(n, 0);
        // 初始化
        dp[n-1] = 1;
        for(int i =n - 2; i >= 0; --i)
        {
            if(i + jump[i] >= n)  // 能够直接弹出
                dp[i] = 1;  
            else // 不能直接弹出，先跳到i+jump[i]，再从i+jump[i]跳出
                dp[i] = dp[i + jump[i]] + 1; 
            //状态转移方程
            // 遍历当前位置更新后影响到的后面的位置
            for(int j = i + 1; j < n; ++j)
                dp[j] =min(dp[j], dp[i] + 1);
        }
        return  dp[0];
    }
};
```

dp[j]>=dp[i]+1  剪枝
```c++
class Solution {
public:
    int minJump(vector<int>& jump) {
        int n = jump.size();
        // 定义状态：dp数组的含义为第i个弹簧到弹出所需的最小次数
        vector<int> dp(n, 0);
        // 初始化
        dp[n-1] = 1;
        for(int i =n - 2; i >= 0; --i)
        {
            if(i + jump[i] >= n)
                dp[i] = 1;
            else
                dp[i] = dp[i + jump[i]] + 1;
            //遍历当前位置更新后影响到的后面的位置，若dp[j] < dp[i]+1 时大于j的元素可以跳到j，然后再跳出,所以不用继续往后遍历
            for(int j = i + 1; j < n && dp[j] >= dp[i]+1; ++j)
                dp[j] = dp[i] + 1;
        }
        return  dp[0];
    }
};
```

2.bfs
```c++
class Solution {
public:
    int minJump(vector<int>& jump) {
        int n=jump.size();
        vector<int> vis(n,0);
        queue<int> q;
        queue<int> num;
        //把0~n-1放入num
        for(int i=0;i<n;i++)
        {
            num.push(i);
        }
        //从0开始
        q.push(0);
        vis[0]=1;
        
        int step=0;
        //bfs
        while(!q.empty())
        {
            int size=q.size();
            while(size>0)
            {
                int tmp=q.front();
                q.pop();
                //是否结束
                if(tmp+jump[tmp]>=n)
                    return step+1;
                //向右找
                if(vis[tmp+jump[tmp]]==0)
                {
                    vis[tmp+jump[tmp]]=1;
                    q.push(tmp+jump[tmp]);
                }
                //向左找
                while(!num.empty() && num.front()<tmp)
                {
                    int top=num.front();
                    num.pop();
                    if(vis[top]==0)
                    {
                        vis[top]=1;
                        q.push(top);
                    }
                }
                size--;
            }
            ++step;
        }
        return -1;
    }
};
```

##  [LCP 10. 二叉树任务调度](https://leetcode-cn.com/problems/er-cha-shu-ren-wu-diao-du/)

### 题目

任务调度优化是计算机性能优化的关键任务之一。在任务众多时，不同的调度策略可能会得到不同的总体执行时间，因此寻求一个最优的调度方案是非常有必要的。

通常任务之间是存在依赖关系的，即对于某个任务，你需要先完成他的前导任务（如果非空），才能开始执行该任务。我们保证任务的依赖关系是一棵二叉树，其中 root 为根任务，root.left 和 root.right 为他的两个前导任务（可能为空），root.val 为其自身的执行时间。

在一个 CPU 核执行某个任务时，我们可以在任何时刻暂停当前任务的执行，并保留当前执行进度。在下次继续执行该任务时，会从之前停留的进度开始继续执行。暂停的时间可以不是整数。

现在，系统有两个 CPU 核，即我们可以同时执行两个任务，但是同一个任务不能同时在两个核上执行。给定这颗任务树，请求出所有任务执行完毕的最小时间。

示例1：

![](https://pic.leetcode-cn.com/3522fbf8ce4ebb20b79019124eb9870109fdfe97fe9da99f6c20c07ceb1c60b3-image.png)

```
输入：root = [47, 74, 31]

输出：121

解释：根节点的左右节点可以并行执行31分钟，剩下的43+47分钟只能串行执行，因此总体执行时间是121分钟。
```

示例2：

![](https://pic.leetcode-cn.com/13accf172ee4a660d241e25901595d55b759380b090890a17e6e7bd51a143e3f-image.png)
```
输入：root = [15, 21, null, 24, null, 27, 26]

输出：87
```

示例3：

![](https://pic.leetcode-cn.com/bef743a12591aafb9047dd95d335b8083dfa66e8fdedc63f50fd406b4a9d163a-image.png)
```
输入：root = [1,3,2,null,null,4,4]

输出：7.5
```

### 题意概述
有一个二叉树形式的任务依赖结构，我们有两个 CPU 核，这两个核可以同时执行不同的任务，问执行完所有任务的最小时间，也即是希望两个 CPU 核的并行时间尽可能大。


### 思路
解题思路主要是参考lee215的微信公众号[2020力扣杯](https://mp.weixin.qq.com/s/rmwVuDpbQlhoK7DcD715bg)

### 代码
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    pair<double,double> dfs(TreeNode * root){
        if(root == NULL) return {0,0};
        pair<double,double> l = dfs(root->left);
        pair<double,double> r = dfs(root->right);
        double s = 0.0;
        double d = 0.0;
        double delta = 0.0;

       if(l.first >= r.first){
            delta = min((l.first-r.first)/2,r.second);
            d = l.second + r.second + r.first + delta;
            s = l.first - r.first - 2*delta;
            return {s + root->val,d};
        }else{
            delta = min((r.first-l.first)/2,l.second);
            d = l.second + r.second + l.first + delta;
            s = r.first - l.first - 2*delta;
            return {s + root->val,d};
        }
    }
    double minimalExecTime(TreeNode* root) {
        pair<double,double> t = dfs(root);
        return t.first + t.second;
    }
};
```