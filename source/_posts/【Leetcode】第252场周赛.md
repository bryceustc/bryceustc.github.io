---
title: 【Leetcode】第252场周赛
date: 2021-08-01 11:19:45
tags: LeetCode
categories: LeetCode
---

### [5830. 三除数](https://leetcode-cn.com/contest/weekly-contest-252/problems/three-divisors/)

给你一个整数 n 。如果 n 恰好有三个正除数 ，返回 true ；否则，返回 false 。

如果存在整数 k ，满足 n = k * m ，那么整数 m 就是 n 的一个 除数 。

<!--more-->
示例1：
```
输入：n = 2
输出：false
解释：2 只有两个除数：1 和 2 。
```

示例2:
```
输入：n = 4
输出：true
解释：4 有三个除数：1、2 和 4 。
```

提示：
- 1 <= n <= 104
### 思路
直接暴力模拟法

### 代码
```c++
class Solution {
public:
    bool isThree(int n) {
        int cnt = 0;
        for (int i = 1; i <= n; ++i) {
            if (n % i == 0) cnt ++;
        }
        return cnt == 3;
    }
};
```

### [5831. 你可以工作的最大周数](https://leetcode-cn.com/contest/weekly-contest-252/problems/maximum-number-of-weeks-for-which-you-can-work/)

给你 n 个项目，编号从 0 到 n - 1 。同时给你一个整数数组 milestones ，其中每个 milestones[i] 表示第 i 个项目中的阶段任务数量。

你可以按下面两个规则参与项目中的工作：

- 每周，你将会完成 某一个 项目中的 恰好一个 阶段任务。你每周都 必须 工作。
在 连续的 两周中，你 不能 参与并完成同一个项目中的两个阶段任务。
- 一旦所有项目中的全部阶段任务都完成，或者仅剩余一个阶段任务都会导致你违反上面的规则，那么你将 停止工作 。注意，由于这些条件的限制，你可能无法完成所有阶段任务。

返回在不违反上面规则的情况下你 最多 能工作多少周。

示例 1：
```
输入：milestones = [1,2,3]
输出：6
解释：一种可能的情形是：
​​​​- 第 1 周，你参与并完成项目 0 中的一个阶段任务。
- 第 2 周，你参与并完成项目 2 中的一个阶段任务。
- 第 3 周，你参与并完成项目 1 中的一个阶段任务。
- 第 4 周，你参与并完成项目 2 中的一个阶段任务。
- 第 5 周，你参与并完成项目 1 中的一个阶段任务。
- 第 6 周，你参与并完成项目 2 中的一个阶段任务。
总周数是 6 。
```

示例 2：
```
输入：milestones = [5,2,1]
输出：7
解释：一种可能的情形是：
- 第 1 周，你参与并完成项目 0 中的一个阶段任务。
- 第 2 周，你参与并完成项目 1 中的一个阶段任务。
- 第 3 周，你参与并完成项目 0 中的一个阶段任务。
- 第 4 周，你参与并完成项目 1 中的一个阶段任务。
- 第 5 周，你参与并完成项目 0 中的一个阶段任务。
- 第 6 周，你参与并完成项目 2 中的一个阶段任务。
- 第 7 周，你参与并完成项目 0 中的一个阶段任务。
总周数是 7 。
注意，你不能在第 8 周参与完成项目 0 中的最后一个阶段任务，因为这会违反规则。
因此，项目 0 中会有一个阶段任务维持未完成状态。
```

提示：
- n == milestones.length
- 1 <= n <= 10^5
- 1 <= milestones[i] <= 10^9

### 思路
贪心: 选择最大与其他任务总和比较,max比sum-max大，必定有任务不能完成。否则，可以完成任务。
### 代码
```c++
class Solution {
public:
    long long numberOfWeeks(vector<int>& milestones) {
        long long sum = 0;
        long long mx = 0;
        long long res = 0;
        for (int &m : milestones) {
            sum += m;
            mx = max(mx, 1LL * m);
        }
        // 如果其余和大于等于最大值，那么一定能做完
        if (sum - mx >= mx) return sum;
        // 否则做不完
        else res = (sum - mx) * 2 + 1;
        return res;
    }
};
```

### [5187. 收集足够苹果的最小花园周长](https://leetcode-cn.com/contest/weekly-contest-252/problems/minimum-garden-perimeter-to-collect-enough-apples/)

给你一个用无限二维网格表示的花园，每一个 整数坐标处都有一棵苹果树。整数坐标 (i, j) 处的苹果树有 |i| + |j| 个苹果。

你将会买下正中心坐标是 (0, 0) 的一块 正方形土地 ，且每条边都与两条坐标轴之一平行。

给你一个整数 neededApples ，请你返回土地的 最小周长 ，使得 至少 有 neededApples 个苹果在土地 里面或者边缘上。

|x| 的值定义为：

- 如果 x >= 0 ，那么值为 x
- 如果 x < 0 ，那么值为 -x

示例 1：
![](https://assets.leetcode.com/uploads/2019/08/30/1527_example_1_2.png)
```
输入：neededApples = 1
输出：8
解释：边长长度为 1 的正方形不包含任何苹果。
但是边长为 2 的正方形包含 12 个苹果（如上图所示）。
周长为 2 * 4 = 8 。
```

示例 2：
```
输入：neededApples = 13
输出：16
```
示例 3：
```
输入：neededApples = 1000000000
输出：5040
```


提示：
- 1 <= neededApples <= 10^15

### 思路

堆优化版本的Dijkstra算法 + 拓扑排序。

路径上需要满足dis[i] < dis[j]，先利用Dijkstra算法求出最短路之后就是构建有向拓扑图一个dag图上求两点之间的路径数量。拓扑排序参考[这里](https://www.cxyxiaowu.com/1084.html)

### 代码
```c++
class Solution {
public:
    struct Node{
        int u, w;
        bool operator < (const Node other)  const{
            return w > other.w;
        }
        Node(int _u, int _w): u(_u), w(_w) {}
    };
    
    #define maxn  20005
    vector<pair<int, int> > G[20005];
    vector<int> G2[maxn], G3[maxn];
    int ind[maxn];
    void add_edge(int u, int v, int w) {
        G[u].push_back({v, w});
        G[v].push_back({u, w});
    }
    
    void add_edge_2(int u, int v) {
        // cout << u << " " << v << endl;
        G2[u].push_back(v);
        // G3[v].push_back(u);
        ind[v]++; // 入度+1
    }
    
    int dis[200005];
    
    
    void dijkstra(int st) {
        priority_queue<Node> pq;
        for (int i = 1; i <= st; ++i) {
            dis[i] = 0x3f3f3f3f;
        }
        dis[st] = 0;
        pq.push(Node(st, 0));
        while (!pq.empty()) {
            auto tmp = pq.top(); pq.pop();
            int u = tmp.u;
            for (auto e: G[u]) {
                auto v = e.first;
                auto w = e.second;
                if (dis[v] > dis[u] + w) {
                    dis[v] = dis[u] + w;
                    pq.push(Node(v, dis[v]));
                }
            }
        }
    }
    int dp[maxn];
    #define mod 1000000007

    int countRestrictedPaths(int n, vector<vector<int>>& edges) {
        for (auto& edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int w = edge[2];
            add_edge(u, v, w);
            add_edge(v, u, w);
        }
        
        //  求最短路
        dijkstra(n);
        
        // 第二部分开始构图
        memset(ind, 0, sizeof(ind));
        for (auto &edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            if (dis[u] > dis[v]) {
                add_edge_2(u, v);
            } else if (dis[v] > dis[u]) {
                add_edge_2(v, u);
            }
        }
        
        // 拓扑排序部分
        memset(dp, 0, sizeof(dp));
        dp[1] = 1;
        queue<int> q;
        for (int i = 1; i <= n; ++i) {
            if (ind[i] == 0) q.push(i);  // 入度为 0 点开始
        }
        while (!q.empty()) {
            auto u = q.front(); q.pop();
            for (auto v: G2[u]) {
                if (--ind[v] == 0) {
                    q.push(v);
                }
                // dp[i] 表示从i到1点的路径数量
                dp[v] = (dp[v] + dp[u]) % mod;
            }
        }
        return dp[n];
    }
};
```

### [5833. 统计特殊子序列的数目](https://leetcode-cn.com/contest/weekly-contest-252/problems/count-number-of-special-subsequences/)

特殊序列 是由 正整数 个 0 ，紧接着 正整数 个 1 ，最后 正整数 个 2 组成的序列。

比方说，[0,1,2] 和 [0,0,1,1,1,2] 是特殊序列。
相反，[2,1,0] ，[1] 和 [0,1,2,0] 就不是特殊序列。
给你一个数组 nums （仅 包含整数 0，1 和 2），请你返回 不同特殊子序列的数目 。由于答案可能很大，请你将它对 109 + 7 取余 后返回。

一个数组的 子序列 是从原数组中删除零个或者若干个元素后，剩下元素不改变顺序得到的序列。如果两个子序列的 下标集合 不同，那么这两个子序列是 不同的 。

示例 1：
```
输入：nums = [0,1,2,2]
输出：3
解释：特殊子序列为 [0,1,2,2]，[0,1,2,2] 和 [0,1,2,2] 。
```

示例 2：
```
输入：nums = [2,2,0,0]
输出：0
解释：数组 [2,2,0,0] 中没有特殊子序列。
```

示例3：
```
输入：nums = [0,1,2,0,1,2]
输出：7
解释：特殊子序列包括：
- [0,1,2,0,1,2]
- [0,1,2,0,1,2]
- [0,1,2,0,1,2]
- [0,1,2,0,1,2]
- [0,1,2,0,1,2]
- [0,1,2,0,1,2]
- [0,1,2,0,1,2]
```

提示：
- 1 <= nums.length <= 10^5
- 0 <= nums[i] <= 2

### 思路
首先可以推导出一个性质。

根据 XOR([1,k]) = 0 且 XOR([2,k+1]) = 0，可以得到最终 [1] = [k+1]。因为这两个0异或就是0，那么证明这两个相等。
推而广之，[i] = [i+k]。

按余数分组进行动态规划

### 代码
```c++
const int INF = 0x3f3f3f3f;

class Solution {
public:
    int minChanges(vector<int>& nums, int k) {
        int n = nums.size();
        vector<unordered_map<int, int>> groups(k);
        vector<int> size(k);
        for (int i = 0; i < k; ++i) {
            for (int j = i; j < n; j += k) {
                groups[i][nums[j]]++;
                size[i]++;
            }
        }
        //异或值为random，需要操作的最小次数
        vector<int> dp(1 << 10, INF);
        dp[0] = 0;
        //从左到右，考虑K区间的index i的处理
        //即在nums中所有映射在区间k中index 为 i的集合的元素修改方法
        for (int i = 0; i < k; ++i) {
            //讨论第一种情况，即i对应集合的值都修改为一个不存在该集合中的值，那么这个集合就要修改size[i]次
            /**
            dp的涵义：在当前index前，异或一个值需要操作的最小次数
            eg: 
            1. 此时i = 3, 在操作前三个集合，即[0, 1, 2]后，会获取多个异或值，并且都是此时获取该异或值的最小操作次数
            2. 要想修改i=3的集合后，操作最小，就在之前获取异或值中最小操作次数的基础上 + size[i];
                比如dp中此时获得55需要的次数最小，那么之后就以这个为基础，在 i=3对应的集合上修改
                修改后可以为任意值， 比如为获取99，只需将i=3的区间所有值都修改为 99^55就好
            */
            int lo = *min_element(dp.begin(), dp.end());
            vector<int> ndp(1 << 10, lo + size[i]);


            // 讨论第二种情况
            /**
            1. 之前考虑的都是集合i中元素都修改为集合i中没有的数字，那么对于集合i而言，它的修改次数是固定的，
                    所以只要求之前需要修改的次数的最小值就可以获取最小值
            2. 可是存在另一种情况，即修改值存在集合i中，那么对于集合i修改的次数就是变量，为 size[i] - group[i][val]
                1. 在修改集合i的次数值为变值时，就需要枚举全部情况
                2. 以val^val`为例
                    1. 因为可能存在 之前集合操作变成 val`的次数不是最小，但是加上size[i] - group[i][val]后
                        其值比 size[i] + min(dp[random])要小
            */
            for (int j = 0; j < (1 << 10); ++j) {
                if (dp[j] == INF)
                    continue;
                for (auto [p, freq] : groups[i]) {
                    int nxt = p ^ j;
                    ndp[nxt] = min(ndp[nxt], dp[j] + size[i] - freq);
                }
            }
            // 将ndp copy给dp，表示操作完i集合后，异或得到每个可能的值需要的最小操作数
            dp = move(ndp);
        }
        //从集合0开始到集合k-1，所有集合都讨论处理后，dp[0]就是获取0需要操作的最小次数
        return dp[0];
    }
};
```