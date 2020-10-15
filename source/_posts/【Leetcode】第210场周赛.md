---
title: 【Leetcode】第210场周赛
date: 2020-10-11 12:32:02
vtags: LeetCode
categories: LeetCode
mathjax: true
---
这次周赛做的还可以，名次算是创了新高，希望有一天能杀入前200吧。加油！
![](1.png)
### [1614. 括号的最大嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-the-parentheses/)

如果字符串满足一下条件之一，则可以称之为 有效括号字符串（valid parentheses string，可以简写为 VPS）：

字符串是一个空字符串 ""，或者是一个不为 "(" 或 ")" 的单字符。
字符串可以写为 AB（A 与 B 字符串连接），其中 A 和 B 都是 有效括号字符串 。
字符串可以写为 (A)，其中 A 是一个 有效括号字符串 。
类似地，可以定义任何有效括号字符串 S 的 嵌套深度 depth(S)：

depth("") = 0
depth(A + B) = max(depth(A), depth(B))，其中 A 和 B 都是 有效括号字符串
depth("(" + A + ")") = 1 + depth(A)，其中 A 是一个 有效括号字符串
例如：""、"()()"、"()(()())" 都是 有效括号字符串（嵌套深度分别为 0、1、2），而 ")(" 、"(()" 都不是 有效括号字符串 。

给你一个 有效括号字符串 s，返回该字符串的 s 嵌套深度 。
<!--more-->
示例1：
```
输入：s = "(1+(2*3)+((8)/4))+1"
输出：3
解释：数字 8 在嵌套的 3 层括号中。
```

示例2:
```
输入：s = "(1)+((2))+(((3)))"
输出：3
```

示例3：
```
输入：s = "1+(2*3)/(2-1)"
输出：1
```

示例4：
```
输入：s = "1"
输出：0
```

提示：
- 1 <= s.length <= 100
- s 由数字 0-9 和字符 '+'、'-'、'*'、'/'、'('、')' 组成
- 题目数据保证括号表达式 s 是 有效的括号表达式

### 思路

理解题意之后直接暴力循环模拟即可。就是在遇到右括号之前有多少左括号，取最大的值即可。

### 代码
```c++
class Solution {
public:
    int maxDepth(string s) {
        int res = 0;
        if (s.empty()) return 0;
        stack<char> stk;
        for (char c : s) {
            if (c == '(') stk.push(c);
            else if (c == ')' && !stk.empty()) {
                res = max(res, (int)stk.size());
                stk.pop();
            }
        }
        return res;
    }
};
```

### [1615. 最大网络秩](https://leetcode-cn.com/problems/maximal-network-rank/)

n 座城市和一些连接这些城市的道路 roads 共同组成一个基础设施网络。每个 roads[i] = [ai, bi] 都表示在城市 ai 和 bi 之间有一条双向道路。

两座不同城市构成的 城市对 的 网络秩 定义为：与这两座城市 直接 相连的道路总数。如果存在一条道路直接连接这两座城市，则这条道路只计算 一次 。

整个基础设施网络的 最大网络秩 是所有不同城市对中的 最大网络秩 。

给你整数 n 和数组 roads，返回整个基础设施网络的 最大网络秩 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/ex1.png)
```
输入：n = 4, roads = [[0,1],[0,3],[1,2],[1,3]]
输出：4
解释：城市 0 和 1 的网络秩是 4，因为共有 4 条道路与城市 0 或 1 相连。位于 0 和 1 之间的道路只计算一次。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/ex2.png)
```
输入：n = 5, roads = [[0,1],[0,3],[1,2],[1,3],[2,3],[2,4]]
输出：5
解释：共有 5 条道路与城市 1 或 2 相连。
```

示例3：
```
输入：n = 8, roads = [[0,1],[1,2],[2,3],[2,4],[5,6],[5,7]]
输出：5
解释：2 和 5 的网络秩为 5，注意并非所有的城市都需要连接起来。
```

提示：
- 2 <= n <= 100
- 0 <= roads.length <= n * (n - 1) / 2
- roads[i].length == 2
- 0 <= ai, bi <= n-1
- ai != bi
- 每对城市之间 最多只有一条 道路相连

### 思路
直接按照题意做就可以。

### 代码
```c++
// 提交代码
class Solution {
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) {
        int res = 0;
        unordered_map<int, unordered_set<int>> m;
        for (auto &v : roads) {
            m[v[0]].insert(v[1]);
            m[v[1]].insert(v[0]);
        }
        for (int i = 0; i <= n; i++) {
            for (int j = i + 1; j <= n; j++) {
                int temp = m[i].size() + m[j].size();
                if (m[i].count(j) && m[j].count(i)) {
                    temp -= 1;
                }
                res = max (res, temp);
            }
        }
        return res;
    }
};
```


```c++
const int MAXN=100;
int has[MAXN][MAXN];
int cnt[MAXN];
class Solution {
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) {
        memset(has,0,sizeof(has));
        memset(cnt,0,sizeof(cnt));
        for (vector<int> &i:roads)
        {
            ++cnt[i.at(0)];
            ++cnt[i.at(1)];
            has[i.at(0)][i.at(1)]=1;
            has[i.at(1)][i.at(0)]=1;
        }
        int ans=0;
        for (int i=0;i<n;++i)
            for (int j=i+1;j<n;++j)
            {
                int c=cnt[i]+cnt[j];
                if (has[i][j]==1)
                    --c;
                ans=max(ans,c);
            }
        return ans;
    }
};
```

### [1616. 分割两个字符串得到回文串](https://leetcode-cn.com/problems/split-two-strings-to-make-palindrome/)

给你两个字符串 a 和 b ，它们长度相同。请你选择一个下标，将两个字符串都在 相同的下标 分割开。由 a 可以得到两个字符串： aprefix 和 asuffix ，满足 a = aprefix + asuffix ，同理，由 b 可以得到两个字符串 bprefix 和 bsuffix ，满足 b = bprefix + bsuffix 。请你判断 aprefix + bsuffix 或者 bprefix + asuffix 能否构成回文串。

当你将一个字符串 s 分割成 sprefix 和 ssuffix 时， ssuffix 或者 sprefix 可以为空。比方说， s = "abc" 那么 "" + "abc" ， "a" + "bc" ， "ab" + "c" 和 "abc" + "" 都是合法分割。

如果 能构成回文字符串 ，那么请返回 true，否则返回 false 。

请注意， x + y 表示连接字符串 x 和 y 。

示例 1：
```
输入：a = "x", b = "y"
输出：true
解释：如果 a 或者 b 是回文串，那么答案一定为 true ，因为你可以如下分割：
aprefix = "", asuffix = "x"
bprefix = "", bsuffix = "y"
那么 aprefix + bsuffix = "" + "y" = "y" 是回文串。
```

示例 2：
```
输入：a = "ulacfd", b = "jizalu"
输出：true
解释：在下标为 3 处分割：
aprefix = "ula", asuffix = "cfd"
bprefix = "jiz", bsuffix = "alu"
那么 aprefix + bsuffix = "ula" + "alu" = "ulaalu" 是回文串。
```

提示：
- 1 <= a.length, b.length <= 105
- a.length == b.length
- a 和 b 都只包含小写英文字母

### 思路

check两次

### 代码

```c++
class Solution {
public:
    bool checkPalindromeFormation(string a, string b) {
        return checkFirst(a,b) || checkFirst(b,a);
    }

    bool checkFirst(string & a,string & b){
        int l=0,r=a.size()-1;

        while(l<=r && a[l]==b[r]){
            l++;
            r--;
        }

        return checkSecond(a,l,r) || checkSecond(b,l,r);
    }

    bool checkSecond(string & s,int l,int r){
        while(l<=r){
            if(s[l]!=s[r]) return false;
            l++;
            r--;
        }

        return true;
    }
};
```

### [1617. 统计子树中城市之间最大距离](https://leetcode-cn.com/problems/count-subtrees-with-max-distance-between-cities/)

给你 n 个城市，编号为从 1 到 n 。同时给你一个大小为 n-1 的数组 edges ，其中 edges[i] = [ui, vi] 表示城市 ui 和 vi 之间有一条双向边。题目保证任意城市之间只有唯一的一条路径。换句话说，所有城市形成了一棵 树 。

一棵 子树 是城市的一个子集，且子集中任意城市之间可以通过子集中的其他城市和边到达。两个子树被认为不一样的条件是至少有一个城市在其中一棵子树中存在，但在另一棵子树中不存在。

对于 d 从 1 到 n-1 ，请你找到城市间 最大距离 恰好为 d 的所有子树数目。

请你返回一个大小为 n-1 的数组，其中第 d 个元素（下标从 1 开始）是城市间 最大距离 恰好等于 d 的子树数目。

请注意，两个城市间距离定义为它们之间需要经过的边的数目。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/p1.png)
```
输入：n = 4, edges = [[1,2],[2,3],[2,4]]
输出：[3,4,0]
解释：
子树 {1,2}, {2,3} 和 {2,4} 最大距离都是 1 。
子树 {1,2,3}, {1,2,4}, {2,3,4} 和 {1,2,3,4} 最大距离都为 2 。
不存在城市间最大距离为 3 的子树。
```

示例 2：
```
输入：n = 2, edges = [[1,2]]
输出：[1]
```

示例3：
```
输入：n = 3, edges = [[1,2],[2,3]]
输出：[2,1]
```

提示：
- 2 <= n <= 15
- edges.length == n-1
- edges[i].length == 2
- 1 <= ui, vi <= n
- 题目保证 (ui, vi) 所表示的边互不相同。

### 思路

树形dp问题

### 代码
```c++
/*
 * 因为n<=15，所以就往状压dp去想
   但是本题之间状态的转移是转移不了的，但是复杂度的思路是对的2^15
   所以想法：二进制压缩状态，枚举所有情况
 * 本题的树，可以看成是无向无环联通图（二者等价）
 * 枚举状态，要首先判断是否无效
   1. 该状态只有1个节点，因为题目要求的d最小是1，即距离为1，所以可以剪枝
   2. 该状态下不是一个子树，即不是一个联通图，那怎么判断是否是联通图？
      通过bfs或dfs判断能到达的节点的个数，是否和集合内元素个数(二进制下1的个数)相同即可
 * 判断了该状态是有效后，如何求最大距离？
   最大距离，其实就是无向无环联通图的最大直径，lc有原题：树的直径、二叉树的直径啊都是一样的
   1. 任选一节点作为开始节点a，去bfs找到离该节点最远的节点b
   2. 从节点b开始，去bfs找到离b最远的节点的c
   3. b与c之间的距离就是该状态下最大距离，即该(子)图的最大直接
 * 注意本题是序号1开始的，所以空间要多开一位
 * 总体复杂度在O(2^n*(n+n))
 */
const int MAXN=15+1;
int vis[MAXN];
class Solution {
    vector<vector<int>> edges;
public:
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& _edges) {
        edges=vector<vector<int>>(n+1);
        for (vector<int> &i:_edges)
        {
            edges.at(i.at(0)).push_back(i.at(1));
            edges.at(i.at(1)).push_back(i.at(0));
        }
        int ALL=(1<<n)-1;
        vector<int> ans(n-1,0);
        for (int i=1;i<=ALL;++i)
        {
            bitset<MAXN> bs(i);
            int cnt_node=bs.count();
            if (cnt_node<=1)
                continue;
            int start_node;
            for (int j=0;j<n;++j)
                if (bs[j]==1)
                {
                    start_node=j+1;
                    break;
                }
            int start_link_cnt=0;
            int diameter=-1;
            bfs(start_node,bs,start_link_cnt,diameter);
            if (start_link_cnt!=cnt_node)
                continue;
            start_link_cnt=0;
            diameter=-1;
            bfs(start_node,bs,start_link_cnt,diameter);
            ++ans.at(diameter-1);
        }
        return ans;
    }
    void bfs(int &start_node,bitset<MAXN> & bs,int &start_link_cnt,int &diameter)
    {
        memset(vis,0,sizeof(vis));
        queue<int> que;
        que.push(start_node);
        vis[start_node]=1;
        int last_node=start_node;
        while (!que.empty())
        {
            int size=que.size();
            start_link_cnt+=size;
            ++diameter;
            for (int i=0;i<size;++i)
            {
                int cur=que.front();
                que.pop();
                last_node=cur;
                for (int nxt:edges.at(cur))
                    if (vis[nxt]==0 && bs[nxt-1]==1)
                    {
                        vis[nxt]=1;
                        que.push(nxt);
                    }
            }
        }
        start_node=last_node;
    }
};
```