---
title: 【Leetcode】第224场周赛
date: 2021-01-17 12:16:41
tags: LeetCode
categories: LeetCode
mathjax: true
---

第224场周赛，排名559，今天迟到，简单题做的时候有些着急，没看清返回条件，下次注意。细心谨慎一点。

![](1.png)

### [5653. 可以形成最大正方形的矩形数目](https://leetcode-cn.com/problems/number-of-rectangles-that-can-form-the-largest-square/)

给你一个数组 rectangles ，其中 rectangles[i] = [li, wi] 表示第 i 个矩形的长度为 li 、宽度为 wi 。

如果存在 k 同时满足 k <= li 和 k <= wi ，就可以将第 i 个矩形切成边长为 k 的正方形。例如，矩形 [4,6] 可以切成边长最大为 4 的正方形。

设 maxLen 为可以从矩形数组 rectangles 切分得到的 最大正方形 的边长。

返回可以切出边长为 maxLen 的正方形的矩形 数目 。

<!--more-->
示例1：
```
输入：rectangles = [[5,8],[3,9],[5,12],[16,5]]
输出：3
解释：能从每个矩形中切出的最大正方形边长分别是 [5,3,5,5] 。
最大正方形的边长为 5 ，可以由 3 个矩形切分得到。
```

示例2:
```
输入：rectangles = [[2,3],[3,7],[4,3],[3,7]]
输出：3
```

提示：
- 1 <= rectangles.length <= 1000
- rectangles[i].length == 2
- 1 <= li, wi <= 109
- li != wi
### 思路
模拟法，注意返回值，

### 代码
```c++
class Solution {
public:
    int countGoodRectangles(vector<vector<int>>& rectangles) {
        int n = rectangles.size();
        unordered_map<int, int> m;
        int res = 0;
        for (int i = 0; i < n; i++) {
            int a = rectangles[i][0];
            int b = rectangles[i][1];
            int c = min(a, b);
            m[c]++;
        }
        for (auto &p : m) {
            res = max(res, p.first);
        }
        res = m[res];
        return res;
    }
};
```

### [5243. 同积元组](https://leetcode-cn.com/problems/tuple-with-same-product/)

给你一个由 不同 正整数组成的数组 nums ，请你返回满足 a * b = c * d 的元组 (a, b, c, d) 的数量。其中 a、b、c 和 d 都是 nums 中的元素，且 a != b != c != d 。

示例 1：
```
输入：nums = [2,3,4,6]
输出：8
解释：存在 8 个满足题意的元组：
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (3,4,2,6) , (3,4,6,2) , (4,3,6,2)
```

示例 2：
```
输入：nums = [1,2,4,5,10]
输出：16
解释：存在 16 个满足题意的元组：
(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2)
(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1)
(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,4,5)
(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)
```

示例 3：
```
输入：nums = [2,3,4,6,8,12]
输出：40
```

示例 4：
```
输入：nums = [2,3,5,7]
输出：0
```

提示：
- 1 <= nums.length <= 1000
- 1 <= nums[i] <= 10^4
- nums 中的所有元素 互不相同

### 思路

### 代码
yxc学习代码
```c++
class Solution {
public:
    int tupleSameProduct(vector<int>& nums) {
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); i ++ )
            for (int j = i + 1; j < nums.size(); j ++ )
                hash[nums[i] * nums[j]] ++ ;
        int res = 0;
        for (auto& [k, v]: hash)
            res += v * (v - 1) / 2 * 8;
        return res;
    }
};
```

比赛提交代码，组合排列那个有些复杂了其实
```c++
typedef long long ll;
class Solution {
public:
    int tupleSameProduct(vector<int>& nums) {
        int res = 0;
        int n = nums.size();
        unordered_map<ll,ll> m;
        for (int i = 0; i < n; i++) {
            for (int j = i+1; j < n; j++) {
                ll x = nums[i] * nums[j];
                m[x]++;
            }
        }
        for (auto &p : m){
            ll tmp = combo(p.second, 2);
            res += tmp * 8;
        }
        return res;
    }
    ll factorial(ll n)
    {
        ll fc=1;
        for(int i=1;i<=n;++i) fc *= i;
        return fc;
    }
    int combo(int n,int m)
    {
        int com=factorial(n)/(factorial(m)*factorial(n-m));
        return com;
    }
};
```

### [5655. 重新排列后的最大子矩阵](https://leetcode-cn.com/problems/largest-submatrix-with-rearrangements/)

给你一个二进制矩阵 matrix ，它的大小为 m x n ，你可以将 matrix 中的 列 按任意顺序重新排列。

请你返回最优方案下将 matrix 重新排列后，全是 1 的子矩阵面积。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/screenshot-2020-12-30-at-40536-pm.png)
```
输入：matrix = [[0,0,1],[1,1,1],[1,0,1]]
输出：4
解释：你可以按照上图方式重新排列矩阵的每一列。
最大的全 1 子矩阵是上图中加粗的部分，面积为 4 。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/screenshot-2020-12-30-at-40852-pm.png)
```
输入：matrix = [[1,0,1,0,1]]
输出：3
解释：你可以按照上图方式重新排列矩阵的每一列。
最大的全 1 子矩阵是上图中加粗的部分，面积为 3 。
```

示例3：
```
输入：matrix = [[1,1,0],[1,0,1]]
输出：2
解释：由于你只能整列整列重新排布，所以没有比面积为 2 更大的全 1 子矩形。
```

示例4：
```
输入：matrix = [[0,0],[0,0]]
输出：0
解释：由于矩阵中没有 1 ，没有任何全 1 的子矩阵，所以面积为 0 。
```

提示：
-  == matrix.length
- n == matrix[i].length
- 1 <= m * n <= 105
- matrix[i][j] 要么是 0 ，要么是 1 。

### 思路

类似[最大1的最大子矩阵](https://blog.csdn.net/zuzhiang/article/details/78693421), 原题利用单调栈来求，此题因为可以随便换列，可以利用排序然后求最大值即可。

### 代码
```c++
class Solution {
public:
    int largestSubmatrix(vector<vector<int>>& matrix) {
        int res = 0;
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> h(n,0); // 存储每列连续1的个数
        vector<int> a(n,0);
        for (int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    h[j]++;
                }
                else {
                    h[j] = 0;
                }
                a[j] = h[j];
            }
            // 降序排列
            sort(a.begin(), a.end());
            reverse(a.begin(), a.end());
            for (int j = 0; j < n; j++) {
                // cout << a[j] << endl;
                res = max(res, (j+1) * a[j]);
            }
        }
         
        return res;
    }
};
```

### [5529. 猫和老鼠 II](https://leetcode-cn.com/contest/weekly-contest-224/problems/cat-and-mouse-ii/)

一只猫和一只老鼠在玩一个叫做猫和老鼠的游戏。

它们所处的环境设定是一个 rows x cols 的方格 grid ，其中每个格子可能是一堵墙、一块地板、一位玩家（猫或者老鼠）或者食物。

- 玩家由字符 'C' （代表猫）和 'M' （代表老鼠）表示。
- 地板由字符 '.' 表示，玩家可以通过这个格子。
- 墙用字符 '#' 表示，玩家不能通过这个格子。
- 食物用字符 'F' 表示，玩家可以通过这个格子。
- 字符 'C' ， 'M' 和 'F' 在 grid 中都只会出现一次。
猫和老鼠按照如下规则移动：

- 老鼠 先移动 ，然后两名玩家轮流移动。
- 每一次操作时，猫和老鼠可以跳到上下左右四个方向之一的格子，他们不能跳到墙所在的格子也不能跳出 grid 。
- catJump 和 mouseJump 是猫和老鼠分别跳一次能到达的最远距离，它们也可以跳小于最大距离的长度。
- 它们可以停留在原地。
- 老鼠可以跳跃过猫的位置。
游戏有 4 种方式会结束：

- 如果猫跟老鼠处在相同的位置，那么猫获胜。
- 如果猫先到达食物，那么猫获胜。
- 如果老鼠先到达食物，那么老鼠获胜。
- 如果老鼠不能在 1000 次操作以内到达食物，那么猫获胜。
给你 rows x cols 的矩阵 grid 和两个整数 catJump 和 mouseJump ，双方都采取最优策略，如果老鼠获胜，那么请你返回 true ，否则返回 false 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/sample_111_1955.png)
```
输入：grid = ["####F","#C...","M...."], catJump = 1, mouseJump = 2
输出：true
解释：猫无法抓到老鼠，也没法比老鼠先到达食物。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/sample_2_1955.png)
```
输入：grid = ["M.C...F"], catJump = 1, mouseJump = 4
输出：true
```

示例3：
```
输入：grid = ["M.C...F"], catJump = 1, mouseJump = 3
输出：false
```

示例4：
```
输入：grid = ["C...#","...#F","....#","M...."], catJump = 2, mouseJump = 5
输出：false
```

示例5：
```
输入：grid = [".M...","..#..","#..#.","C#.#.","...#F"], catJump = 3, mouseJump = 1
输出：true
```

提示：
- rows == grid.length
- cols = grid[i].length
- 1 <= rows, cols <= 8
- grid[i][j] 只包含字符 'C' ，'M' ，'F' ，'.' 和 '#' 。
- grid 中只包含一个 'C' ，'M' 和 'F' 。
- 1 <= catJump, mouseJump <= 8

### 思路
dp + 子集遍历

### 代码
```c++
int dp[13][5000];
int cost[5000];
const int INF = 1e9 + 7;
class Solution {
public:
    int minimumTimeRequired(vector<int>& jobs, int k) {
        int n = jobs.size();
        int mask = (1<<n)-1;
        memset(dp,INF,sizeof(dp));
        memset(cost,0,sizeof(cost));
        
        /*intial*/
        dp[0][0] = 0;
        for(int i = 1; i <= k; ++i) dp[i][0] = 0;
        for(int i = 1; i < (1<<n); ++i){
            for(int j = 0; j < n; ++j){
                if(i&(1<<j)) cost[i] += jobs[j];
            }
            dp[1][i] = cost[i];
        }
        
        for(int i = 2; i <= k; ++i){
            for(int j = 1; j <= mask; ++j){
                for(int l = j; l != 0; l = (l-1)&j){
                    if(dp[i-1][j^l] != INF)
                        dp[i][j] = min(dp[i][j],max(dp[i-1][j^l],cost[l]));
                }
            }
        }
        
        return dp[k][(1<<n)-1];
    }
};
```