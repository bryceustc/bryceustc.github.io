---
title: 【Leetcode】第231场周赛
date: 2021-03-07 11:25:09
tags: LeetCode
categories: LeetCode
mathjax: true
---

时隔一个多月又重新开始做下周赛，只ac了两道，果然很久不动脑子，就锈得的不行。最近也是在忙着写毕业论文，要好好准备毕业，同时每天做一道题，让脑子动起来！！

### [5697. 检查二进制字符串字段](https://leetcode-cn.com/contest/weekly-contest-231/problems/check-if-binary-string-has-at-most-one-segment-of-ones/)

给你一个二进制字符串 s ，该字符串 不含前导零 。

如果 s 最多包含**一个由连续的 '1' 组成的字段**，返回 true​​​ 。否则，返回 false 。

<!--more-->
示例1：
```
输入：s = "1001"
输出：false
解释：字符串中的 1 没有形成一个连续字段。
```

示例2:
```
输入：s = "110"
输出：true
```

提示：
- 1 <= s.length <= 100
- s[i]​​​​ 为 '0' 或 '1'
- s[0] 为 '1'
### 思路
注意 '1'也是符合要求的

### 代码
```c++
class Solution {
public:
    bool checkOnesSegment(string s) {
        int c=s[0]=='1';
        for(int i=1;i<s.size();++i)c+=s[i]=='1' && s[i-1]=='0';
        return c<=1;
    }
};
```

### [5698. 构成特定和需要添加的最少元素](https://leetcode-cn.com/contest/weekly-contest-231/problems/minimum-elements-to-add-to-form-a-given-sum/)

给你一个整数数组 nums ，和两个整数 limit 与 goal 。数组 nums 有一条重要属性：abs(nums[i]) <= limit 。

返回使数组元素总和等于 goal 所需要向数组中添加的**最少元素数量**，添加元素**不应改变**数组中 abs(nums[i]) <= limit 这一属性。

注意，如果 x >= 0 ，那么 abs(x) 等于 x ；否则，等于 -x 。

示例 1：
```
输入：nums = [1,-1,1], limit = 3, goal = -4
输出：2
解释：可以将 -2 和 -3 添加到数组中，数组的元素总和变为 1 - 1 + 1 - 2 - 3 = -4 。
```

示例 2：
```
输入：nums = [1,-10,9,1], limit = 100, goal = 0
输出：1
```

提示：
- 1 <= nums.length <= 105
- 1 <= limit <= 106
- -limit <= nums[i] <= limit
- -109 <= goal <= 109

### 思路
求出数组和和目标之间的差值，然后分类讨论即可。主要是利用了贪心。
### 代码
```c++
class Solution {
public:
    int minElements(vector<int>& nums, int limit, int goal) {
        int res = 0;
        int n = nums.size();
        long long sum = 0;
        for (int &num : nums) {
            sum += num;
        }
        long long delta = abs(goal - sum);
        // 不需要添加元素
        if (delta == 0) return res;
        // 添加一个元素
        if (delta <= limit) {
            res += 1;
            return res;
        } else {
            // 添加多个元素
            res = delta / limit;
            int r = delta % limit;
            if (r != 0) res++;
        }
        return res;
    }
};



class Solution {
public:
    int minElements(vector<int>& nums, int limit, int goal) {
        long long sum=0;
        for(auto x:nums)sum+=x;
        sum=abs(goal-sum);
        return (sum+limit-1)/limit;
    }
};
```

### [5699. 从第一个节点出发到最后一个节点的受限路径数](https://leetcode-cn.com/contest/weekly-contest-231/problems/number-of-restricted-paths-from-first-to-last-node/)

现有一个加权无向连通图。给你一个正整数 n ，表示图中有 n 个节点，并按从 1 到 n 给节点编号；另给你一个数组 edges ，其中每个 edges[i] = [ui, vi, weighti] 表示存在一条位于节点 ui 和 vi 之间的边，这条边的权重为 weighti 。

从节点 start 出发到节点 end 的路径是一个形如 [z0, z1, z2, ..., zk] 的节点序列，满足 z0 = start 、zk = end 且在所有符合 0 <= i <= k-1 的节点 zi 和 zi+1 之间存在一条边。

路径的距离定义为这条路径上所有边的权重总和。用 1tanceToLastNode(x) 表示节点 n 和 x 之间路径的最短距离。**受限路径**为满足 distanceToLastNode(zi) > distanceToLastNode(zi+1) 的一条路径，其中 0 <= i <= k-1 。

返回从节点 1 出发到节点 n 的**受限路径数**。由于数字可能很大，请返回对 109 + 7**取余**的结果。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/07/restricted_paths_ex1.png)
```
输入：n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
输出：3
解释：每个圆包含黑色的节点编号和蓝色的 distanceToLastNode 值。三条受限路径分别是：
1) 1 --> 2 --> 5
2) 1 --> 2 --> 3 --> 5
3) 1 --> 3 --> 5
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/07/restricted_paths_ex22.png)
```
输入：n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
输出：1
解释：每个圆包含黑色的节点编号和蓝色的 distanceToLastNode 值。唯一一条受限路径是：1 --> 3 --> 7 。
```

提示：
- 1 <= n <= 2 * 104
- n - 1 <= edges.length <= 4 * 104
- edges[i].length == 3
- 1 <= ui, vi <= n
- ui != vi
- 1 <= weighti <= 105
- 任意两个节点之间至多存在一条边
- 任意两个节点之间至少存在一条路径

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

### [5700. 使所有区间的异或结果为零](https://leetcode-cn.com/contest/weekly-contest-231/problems/make-the-xor-of-all-segments-equal-to-zero/)

给你一个整数数组 nums​​​ 和一个整数 k​​​​​ 。区间 [left, right]（left <= right）的 异或结果 是对下标位于 left 和 right（包括 left 和 right ）之间所有元素进行 XOR 运算的结果：nums[left] XOR nums[left+1] XOR ... XOR nums[right] 。

返回数组中 要更改的最小元素数 ，以使所有长度为 k 的区间异或结果等于零。

示例 1：
```
输入：nums = [1,2,0,3,0], k = 1
输出：3
解释：将数组 [1,2,0,3,0] 修改为 [0,0,0,0,0]
```

示例 2：
```
输入：nums = [3,4,5,2,1,7,3,4,7], k = 3
输出：3
解释：将数组 [3,4,5,2,1,7,3,4,7] 修改为 [3,4,7,3,4,7,3,4,7]
```

示例3：
```
输入：nums = [1,2,4,1,2,5,1,2,6], k = 3
输出：3
解释：将数组[1,2,4,1,2,5,1,2,6] 修改为 [1,2,3,1,2,3,1,2,3]
```

提示：
- 1 <= k <= nums.length <= 2000
​​​​​​- 0 <= nums[i] < 210

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