---
title: 【Leetcode】第197场周赛
date: 2020-07-12 13:35:58
tags: LeetCode
categories: LeetCode
mathjax: true
---
## 第197场周赛

本周题目依旧AC 2/4，依旧是只做出来2道题，第三题用Dijkstra求最大概率路径，不熟悉这个算法没做出来，第二题字符串的一道题找数学规律找了半天，还是得多努力啊，继续贴排名吧，排名很不理想，希望知耻而后勇。只能说自己是个勤奋的人，但不是个聪明的人。时间不会给你答案，但努力可能会。
![](1.png)

### [5460. 好数对](https://leetcode-cn.com/problems/number-of-good-pairs/)

给你一个整数数组 nums 。

如果一组数字 (i,j) 满足 nums[i] == nums[j] 且 i < j ，就可以认为这是一组 好数对 。

返回好数对的数目。
<!--more-->
示例1：
```
输入：nums = [1,2,3,1,1,3]
输出：4
解释：有 4 组好数对，分别是 (0,3), (0,4), (3,4), (2,5) ，下标从 0 开始
```

示例2:
```
输入：nums = [1,1,1,1]
输出：6
解释：数组中的每组数字都是好数对
```

示例3：
```
输入：nums = [1,2,3]
输出：0
```

提示：
- 1 <= nums.length <= 100
- 1 <= nums[i] <= 100
### 思路

直接双重循环暴力模拟

### 代码
```c++
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        int res = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i+1; j < n; ++j) {
                if (nums[i] == nums[j]) {
                    res++;
                }
            }
        }
        return res;
    }
};
```

### [5461. 仅含 1 的子串数](https://leetcode-cn.com/problems/number-of-substrings-with-only-1s/)

给你一个二进制字符串 s（仅由 '0' 和 '1' 组成的字符串）。

返回所有字符都为 1 的子字符串的数目。

由于答案可能很大，请你将它对 10^9 + 7 取模后返回。


示例 1：
```
输入：s = "0110111"
输出：9
解释：共有 9 个子字符串仅由 '1' 组成
"1" -> 5 次
"11" -> 3 次
"111" -> 1 次
```

示例 2：
```
输入：s = "101"
输出：2
解释：子字符串 "1" 在 s 中共出现 2 次
```

示例3：
```
输入：s = "111111"
输出：21
解释：每个子字符串都仅由 '1' 组成
```

示例4：
```
输入：s = "000"
输出：0
```

提示：
- s[i] == '0' 或 s[i] == '1'
- 1 <= s.length <= 10^5

### 思路
这个题有个规律需要知道，就是连续一段相同字符构成的字符串，比如每一段相邻的'1111',4个1连续相邻，那就是连续会有 (1 + 4) * 4 / 2 = 10中只包含1的子串，因为每组连续可以有1+2+...n种组合，就是利用等差数列求和公式，就是长度*(长度+1)/2。

所以此题思路就是，计算每一段连续1的子串，所能提供的贡献度，然后相加起来。

PS:组合排列公式：

$$
A_{n}^{m}=n(n-1)(n-2) \bullet \bullet(n-m+1)=\frac{n !}{(n-m) !}
$$
$$
C_{n}^{m}=\frac{A_{n}^{m}}{m !}=\frac{n !}{m !(n-m) !} ; \quad \mathrm{C}(\mathrm{n}, \mathrm{m})=\mathrm{C}(\mathrm{n}, \mathrm{n}-\mathrm{m})
$$

### 代码
```c++
class Solution {
public:
    int numSub(string s) {
        int n = s.size();
        int mod = 1e9+7;
        if (n==0) return 0;
        long long int len = 0;
        long long int res = 0;
        for (int i = 0; i < n; ++i) {
            if (s[i]=='1') {
                len++;
            } else{
                res += (len + 1) * len / 2;
                len = 0;
            }
        }
        if (len > 0) {
            res += (len + 1) * len / 2;
        }
        return res%mod;
    }
};
```

### [5454. 概率最大的路径](https://leetcode-cn.com/problems/path-with-maximum-probability/)

给你一个由 n 个节点（下标从 0 开始）组成的无向加权图，该图由一个描述边的列表组成，其中 edges[i] = [a, b] 表示连接节点 a 和 b 的一条无向边，且该边遍历成功的概率为 succProb[i] 。

指定两个节点分别作为起点 start 和终点 end ，请你找出从起点到终点成功概率最大的路径，并返回其成功概率。

如果不存在从 start 到 end 的路径，请 返回 0 。只要答案与标准答案的误差不超过 1e-5 ，就会被视作正确答案。

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex1.png)

```
输入：n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
输出：0.25000
解释：从起点到终点有两条路径，其中一条的成功概率为 0.2 ，而另一条为 0.5 * 0.5 = 0.25
```

示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex2.png)

```
输入：n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
输出：0.30000
```
示例 3：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex3.png)
```
输入：n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
输出：0.00000
解释：节点 0 和 节点 2 之间不存在路径
```

提示：
- 2 <= n <= 10^4
- 0 <= start, end < n
- start != end
- 0 <= a, b < n
- a != b
- 0 <= succProb.length == edges.length <= 2*10^4
- 0 <= succProb[i] <= 1
- 每两个节点之间最多有一条边

### 思路

这个题具体做法就是bfs + 堆。我们对这个图进行bfs计算概率值最大的路径，每次从队列里取出概率当前最大的点来扩展新的路径并标记。这样做到终点即为答案。或者bfs不到终点返回0。因为概率都是越乘越小，所以第一次从优先队列里出来标记个 vi 就好了

也可以用可以用Dijkstra算法。
### 代码
```c++
class Solution {
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        vector<vector<pair<double,int>>> graph(n, vector<pair<double,int>>());
        // 碰到图的题先是要构建图，通过邻接矩阵
        for (int i=0; i < edges.size(); ++i) {
            auto e = edges[i];
            // 无向图，两边都要构建
            graph[e[0]].push_back({succProb[i], e[1]});
            graph[e[1]].push_back({succProb[i], e[0]});
        }
        // 记录是否访问过，其实这里是使用了贪心，因为概率都是越乘越小
        vector<int> visited(n,0);
        // 优先队列默认使用大顶堆
        priority_queue<pair<double,int>> q;
        q.push({1,start});
        // 
        while(!q.empty()) {
            auto p = q.top();
            q.pop();
            double curProb = p.first;
            double curPos = p.second;
            if (visited[curPos]==1) {
                continue;
            }
            visited[curPos] = 1;
            // 找到直接返回
            if (curPos == end) return curProb;
            for (auto next : graph[curPos]){
                double nextProb = next.first;
                int nextPos = next.second;
                if (visited[nextPos]==1) {
                    continue;
                }
                q.push({curProb * nextProb, nextPos});
            }
        }
        // 到这一步是没找到直接返回 0 
        return 0;
    }
};
```