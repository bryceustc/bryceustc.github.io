---
title: 【Leetcode】第212场周赛
date: 2020-10-25 12:39:37
tags: LeetCode
categories: LeetCode
mathjax: true
---
2020.10.25, 第212场周赛
![](1.png)
### [5546. 按键持续时间最长的键](https://leetcode-cn.com/problems/slowest-key/)

LeetCode 设计了一款新式键盘，正在测试其可用性。测试人员将会点击一系列键（总计 n 个），每次一个。

给你一个长度为 n 的字符串 keysPressed ，其中 keysPressed[i] 表示测试序列中第 i 个被按下的键。releaseTimes 是一个升序排列的列表，其中 releaseTimes[i] 表示松开第 i 个键的时间。字符串和数组的 下标都从 0 开始 。第 0 个键在时间为 0 时被按下，接下来每个键都 恰好 在前一个键松开时被按下。

测试人员想要找出按键 持续时间最长 的键。第 i 次按键的持续时间为 releaseTimes[i] - releaseTimes[i - 1] ，第 0 次按键的持续时间为 releaseTimes[0] 。

注意，测试期间，同一个键可以在不同时刻被多次按下，而每次的持续时间都可能不同。

请返回按键 持续时间最长 的键，如果有多个这样的键，则返回 按字母顺序排列最大 的那个键。
<!--more-->
示例1：
```
输入：releaseTimes = [9,29,49,50], keysPressed = "cbcd"
输出："c"
解释：按键顺序和持续时间如下：
按下 'c' ，持续时间 9（时间 0 按下，时间 9 松开）
按下 'b' ，持续时间 29 - 9 = 20（松开上一个键的时间 9 按下，时间 29 松开）
按下 'c' ，持续时间 49 - 29 = 20（松开上一个键的时间 29 按下，时间 49 松开）
按下 'd' ，持续时间 50 - 49 = 1（松开上一个键的时间 49 按下，时间 50 松开）
按键持续时间最长的键是 'b' 和 'c'（第二次按下时），持续时间都是 20
'c' 按字母顺序排列比 'b' 大，所以答案是 'c'
```

示例2:
```
输入：releaseTimes = [12,23,36,46,62], keysPressed = "spuda"
输出："a"
解释：按键顺序和持续时间如下：
按下 's' ，持续时间 12
按下 'p' ，持续时间 23 - 12 = 11
按下 'u' ，持续时间 36 - 23 = 13
按下 'd' ，持续时间 46 - 36 = 10
按下 'a' ，持续时间 62 - 46 = 16
按键持续时间最长的键是 'a' ，持续时间 16
```

提示：
- releaseTimes.length == n
- keysPressed.length == n
- 2 <= n <= 1000
- 0 <= releaseTimes[i] <= 109
- releaseTimes[i] < releaseTimes[i+1]
- keysPressed 仅由小写英文字母组成

### 思路

理解题意之后直接暴力循环模拟即可。注意相同的字符，不同的持续时间，不是叠加的关系。

### 代码
```c++
class Solution {
public:
    char slowestKey(vector<int>& releaseTimes, string keysPressed) {
        char res = keysPressed[0];
        int n = keysPressed.size();
        int t = releaseTimes[0];
        for (int i = 1; i < n; i++) {
            if (t < releaseTimes[i] - releaseTimes[i-1]) {
                t = releaseTimes[i] - releaseTimes[i-1];
                res = keysPressed[i];
            } else if (t == releaseTimes[i] - releaseTimes[i-1]) {
                if (res < keysPressed[i]) {
                    res = keysPressed[i];
                }
            }
        }
        return res;
    }
};
```

### [5547. 等差子数组](https://leetcode-cn.com/problems/arithmetic-subarrays/)

如果一个数列由至少两个元素组成，且每两个连续元素之间的差值都相同，那么这个序列就是 等差数列 。更正式地，数列 s 是等差数列，只需要满足：对于每个有效的 i ， s[i+1] - s[i] == s[1] - s[0] 都成立。

例如，下面这些都是 等差数列 ：

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

下面的数列 不是等差数列 ：
```
1, 1, 2, 5, 7
```

给你一个由 n 个整数组成的数组 nums，和两个由 m 个整数组成的数组 l 和 r，后两个数组表示 m 组范围查询，其中第 i 个查询对应范围 [l[i], r[i]] 。所有数组的下标都是 从 0 开始 的。

返回 boolean 元素构成的答案列表 answer 。如果子数组 nums[l[i]], nums[l[i]+1], ... , nums[r[i]] 可以 重新排列 形成 等差数列 ，answer[i] 的值就是 true；否则answer[i] 的值就是 false 。

示例 1：
```
输入：nums = [4,6,5,9,3,7], l = [0,0,2], r = [2,3,5]
输出：[true,false,true]
解释：
第 0 个查询，对应子数组 [4,6,5] 。可以重新排列为等差数列 [6,5,4] 。
第 1 个查询，对应子数组 [4,6,5,9] 。无法重新排列形成等差数列。
第 2 个查询，对应子数组 [5,9,3,7] 。可以重新排列为等差数列 [3,5,7,9] 。
```

示例 2：
```
输入：nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]
输出：[false,true,false,false,true,true]
```

提示：
- n == nums.length
- m == l.length
- m == r.length
- 2 <= n <= 500
- 1 <= m <= 500
- 0 <= l[i] < r[i] < n
- -105 <= nums[i] <= 105

### 思路
直接按照题意做就可以。

### 代码
```c++
class Solution {
public:
    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) {
        int n = nums.size();
        int m = l.size();
        vector<bool> res(m, 0);
        for (int i = 0; i < m; i++) {
            vector<int> vec(nums.begin() + l[i], nums.begin() + r[i] + 1);
            res[i] = check(vec);
        }
        return res;
    }
    
    bool check(vector<int> &nums) {
        int n = nums.size();
        if (n < 2) return 0;
        sort(nums.begin(), nums.end());
        int d = nums[1] - nums[0];
        for (int i = 1; i < n; i++) {
            if (d != nums[i] - nums[i-1]) {
                return 0;
            }
        }
        return 1;
    }
};
```

### [5548. 最小体力消耗路径](https://leetcode-cn.com/problems/path-with-minimum-effort/)

你准备参加一场远足活动。给你一个二维 rows x columns 的地图 heights ，其中 heights[row][col] 表示格子 (row, col) 的高度。一开始你在最左上角的格子 (0, 0) ，且你希望去最右下角的格子 (rows-1, columns-1) （注意下标从 0 开始编号）。你每次可以往 上，下，左，右 四个方向之一移动，你想要找到耗费 体力 最小的一条路径。

一条路径耗费的 体力值 是路径上相邻格子之间 高度差绝对值 的 最大值 决定的。

请你返回从左上角走到右下角的最小 体力消耗值 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex1.png)
```
输入：heights = [[1,2,2],[3,8,2],[5,3,5]]
输出：2
解释：路径 [1,3,5,3,5] 连续格子的差值绝对值最大为 2 。
这条路径比路径 [1,2,2,2,5] 更优，因为另一条路劲差值最大值为 3 。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex2.png)
```
输入：heights = [[1,2,3],[3,8,4],[5,3,5]]
输出：1
解释：路径 [1,2,3,4,5] 的相邻格子差值绝对值最大为 1 ，比路径 [1,3,5,3,5] 更优。
```

示例 3:
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex3.png)
```
输入：heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
输出：0
解释：上图所示路径不需要消耗任何体力。
```

提示：
- rows == heights.length
- columns == heights[i].length
- 1 <= rows, columns <= 100
- 1 <= heights[i][j] <= 106

### 思路

(二分答案，宽度优先搜索) 

O(mnlogS)

见到最大值最小的描述，直接二分答案。

给定限制 mid，通过 BFS 判断是否能在此限制下到达终点；如果可以，减小 mid；否则增大 mid。

通过二分的方式找到最小可行的 mid 值。

时间复杂度

二分的判定时间为 O(mn)O(mn)，二分的范围为 S，这里 S 为 106。

故总时间复杂度为 O(mnlogS)。

空间复杂度

需要 O(mn) 的额外空间存储 BFS 是数据结构。

### 代码

```c++
class Solution {
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int l = 0, r = 1000000;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid, heights)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
    bool check(int limit, vector<vector<int>>&h) {
        int m = h.size();
        int n = h[0].size();
        int dx[4] = {1,-1,0,0};
        int dy[4] = {0,0,1,-1};
        memset(vis, false, sizeof(vis));

        queue<pair<int,int>> q;
        q.push({0,0});
        vis[0][0] = true;
        while(!q.empty()) {
            int ux = q.front().first;
            int uy = q.front().second;
            if (ux == m - 1 && uy == n - 1) return true;
            q.pop();
            for (int k = 0; k < 4; k++) {
                int vx = ux + dx[k];
                int vy = uy + dy[k];
                if (vx < 0 || vx >= m || vy < 0 || vy >= n) continue;
                // 就是limit是体力限制不能走到这条路，就是二分遍历limit看哪个能够到达目的地
                if (abs(h[vx][vy] - h[ux][uy]) > limit) continue;
                if (!vis[vx][vy]) {
                    vis[vx][vy] = true;
                    q.push({vx,vy});
                }
            }
        }
        return false;
    }
private:
    bool vis[110][110];
};
```

### [1632. 矩阵转换后的秩](https://leetcode-cn.com/problems/rank-transform-of-a-matrix/)

给你一个 m x n 的矩阵 matrix ，请你返回一个新的矩阵 answer ，其中 answer[row][col] 是 matrix[row][col] 的秩。

每个元素的 秩 是一个整数，表示这个元素相对于其他元素的大小关系，它按照如下规则计算：

- 如果一个元素是它所在行和列的最小值，那么它的 秩 为 1。
- 如果两个元素 p 和 q 在 同一行 或者 同一列 ，那么：
    - 如果 p < q ，那么 rank(p) < rank(q)
    - 如果 p == q ，那么 rank(p) == rank(q)
    - 如果 p > q ，那么 rank(p) > rank(q)
- 秩 需要越 小 越好。

题目保证按照上面规则 answer 数组是唯一的。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank1.jpg)
```
输入：matrix = [[1,2],[3,4]]
输出：[[1,2],[2,3]]
解释：
matrix[0][0] 的秩为 1 ，因为它是所在行和列的最小整数。
matrix[0][1] 的秩为 2 ，因为 matrix[0][1] > matrix[0][0] 且 matrix[0][0] 的秩为 1 。
matrix[1][0] 的秩为 2 ，因为 matrix[1][0] > matrix[0][0] 且 matrix[0][0] 的秩为 1 。
matrix[1][1] 的秩为 3 ，因为 matrix[1][1] > matrix[0][1]， matrix[1][1] > matrix[1][0] 且 matrix[0][1] 和 matrix[1][0] 的秩都为 2 。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank2.jpg)
```
输入：matrix = [[7,7],[7,7]]
输出：[[1,1],[1,1]]
```

示例3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank3.jpg)
```
输入：matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]
输出：[[4,2,3],[1,3,4],[5,1,6],[1,3,4]]
```

提示：
- m == matrix.length
- n == matrix[i].length
- 1 <= m, n <= 500
- -109 <= matrix[row][col] <= 109

### 思路

首先，将每行及每列中相等的元素找出来，然后连并查集的边，注意这里不需要连所有边，只要相邻两个连边即可。在后面的过程中，我们只考虑并查集中每一个连通分量的根节点。

接下来，每行每列分别排序，根据排序后的大小关系连拓扑排序的边。同样，这里也不需要连所有边，只要相邻两个大小不等的元素连边即可。

然后，进行拓扑排序。每个点的秩初始都为111，在拓扑排序过程中，如果有一条 u→vu\to vu→v 的边，则将 vvv 的秩设置为 max(rank(v),rank(u)+1)\max(rank(v), rank(u)+1)max(rank(v),rank(u)+1)。其余操作同一般的拓扑排序。

最后，将矩阵中每个点的秩设置为并查集中其所在的连通分量的根节点的秩即可。

### 代码
```c++
class UnionFind {
  int n;
  vector<int> parent, size;

public:
  UnionFind(int n) {
    this->n = n;
    parent = vector<int>(n);
    size = vector<int>(n, 1);
    for (int i = 0; i < n; ++i)
      parent[i] = i;
  }

  int find(int idx) {
    if (parent[idx] == idx)
      return idx;
    return parent[idx] = find(parent[idx]);
  }

  void connect(int a, int b) {
    int fa = find(a), fb = find(b);
    if (fa != fb) {
      if (size[fa] > size[fb]) {
        parent[fb] = fa;
        size[fa] += size[fb];
      } else {
        parent[fa] = fb;
        size[fb] += size[fa];
      }
    }
  }
};

class Solution {
public:
    vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        UnionFind uf(n * m);
        for (int i = 0; i < n; ++i) {
            map<int, vector<int>> mp;
            for (int j = 0; j < m; ++j)
                mp[matrix[i][j]].emplace_back(i * m + j);
            for (auto &[num, vec] : mp) {
                for (int k = 0; k + 1 < vec.size(); ++k)
                    uf.connect(vec[k], vec[k + 1]);
            }
        }
        for (int j = 0; j < m; ++j) {
            map<int, vector<int>> mp;
            for (int i = 0; i < n; ++i)
                mp[matrix[i][j]].emplace_back(i * m + j);
            for (auto &[num, vec] : mp) {
                for (int k = 0; k + 1 < vec.size(); ++k)
                    uf.connect(vec[k], vec[k + 1]);
            }
        }
        vector<vector<int>> adj(n * m);
        vector<int> indegree(n * m);
        for (int i = 0; i < n; ++i) {
            vector<pair<int, int>> v(m);
            for (int j = 0; j < m; ++j)
                v[j] = make_pair(matrix[i][j], j);
            sort(v.begin(), v.end());
            for (int j = 0; j + 1 < m; ++j)
                if (v[j].first != v[j + 1].first) {
                    int uu = uf.find(i * m + v[j].second);
                    int vv = uf.find(i * m + v[j + 1].second);
                    adj[uu].emplace_back(vv);
                    indegree[vv]++;
                }
        }
        for (int j = 0; j < m; ++j) {
            vector<pair<int, int>> v(n);
            for (int i = 0; i < n; ++i)
                v[i] = make_pair(matrix[i][j], i);
            sort(v.begin(), v.end());
            for (int i = 0; i + 1 < n; ++i)
                if (v[i].first != v[i + 1].first) {
                    int uu = uf.find(v[i].second * m + j);
                    int vv = uf.find(v[i + 1].second * m + j);
                    adj[uu].emplace_back(vv);
                    indegree[vv]++;
                }
        }
        vector<int> ans(n * m, 1);
        queue<int> q;
        for (int i = 0; i < n * m; ++i) {
            if (uf.find(i) == i && indegree[i] == 0)
                q.emplace(i);
        }
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            for (int v : adj[u]) {
                ans[v] = max(ans[v], ans[u] + 1);
                indegree[v]--;
                if (indegree[v] == 0)
                    q.emplace(v);
            }
        }
        vector<vector<int>> result(n, vector<int>(m));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                result[i][j] = ans[uf.find(i * m + j)];
        return result;
    }
};
```