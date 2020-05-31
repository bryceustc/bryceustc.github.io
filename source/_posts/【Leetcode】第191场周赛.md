---
title: 【Leetcode】第191场周赛
date: 2020-05-31 13:30:56
tags: LeetCode
categories: LeetCode
---
## 第191场周赛

本周题目AC 2/4，这次题目质量不错，只AC了两道，好好总结，继续加油。


### [5424. 数组中两元素的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-two-elements-in-an-array/)

给你一个整数数组 nums，请你选择数组的两个不同下标 i 和 j，使 (nums[i]-1)*(nums[j]-1) 取得最大值。

请你计算并返回该式的最大值。
<!--more-->

- 如果 searchWord 是某一个单词的前缀，则返回句子 sentence 中该单词所对应的下标（下标从 1 开始）。
- 如果 searchWord 是多个单词的前缀，则返回匹配的第一个单词的下标（最小下标）。
- 如果 searchWord 不是任何单词的前缀，则返回 -1 。

字符串 S 的 「前缀」是 S 的任何前导连续子字符串。
示例1：
```
输入：nums = [3,4,5,2]
输出：12 
解释：如果选择下标 i=1 和 j=2（下标从 0 开始），则可以获得最大值，(nums[1]-1)*(nums[2]-1) = (4-1)*(5-1) = 3*4 = 12 。 
```

示例2:
```
输入：nums = [1,5,4,5]
输出：16
解释：选择下标 i=1 和 j=3（下标从 0 开始），则可以获得最大值 (5-1)*(5-1) = 16 。
```

示例3：
```
输入：nums = [3,7]
输出：12
```
提示：
- 2 <= nums.length <= 500
- 1 <= nums[i] <= 10^3
### 思路

暴力模拟，不存在负数，所以直接选出最大的两个数直接相乘就好，注意不是连续的两个下标，任意两个不同的下标就好

### 代码
```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        int res = 0;
        sort(nums.begin(),nums.end());
        int a = nums[n-1];
        int b = nums[n-2];
        res = (a-1)*(b-1);
        return res;
    }
};

class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int res = 0;
        int n = nums.size();
        for (int i=0;i<n;i++)
        {
            for (int j=i+1;j<n;j++)
            {
                res = max(res, (nums[i]-1)*(nums[j]-1));
            }
        }
        return res;
    }
};
```

### [5425. 切割后面积最大的蛋糕](https://leetcode-cn.com/problems/maximum-area-of-a-piece-of-cake-after-horizontal-and-vertical-cuts/)

矩形蛋糕的高度为 h 且宽度为 w，给你两个整数数组 horizontalCuts 和 verticalCuts，其中 horizontalCuts[i] 是从矩形蛋糕顶部到第  i 个水平切口的距离，类似地， verticalCuts[j] 是从矩形蛋糕的左侧到第 j 个竖直切口的距离。

请你按数组 horizontalCuts 和 verticalCuts 中提供的水平和竖直位置切割后，请你找出 面积最大 的那份蛋糕，并返回其 面积 。由于答案可能是一个很大的数字，因此需要将结果对 10^9 + 7 取余后返回。

示例 1：
![](1.png)
```
输入：h = 5, w = 4, horizontalCuts = [1,2,4], verticalCuts = [1,3]
输出：4 
解释：上图所示的矩阵蛋糕中，红色线表示水平和竖直方向上的切口。切割蛋糕后，绿色的那份蛋糕面积最大。
```

示例 2：
![](2.png)
```
输入：h = 5, w = 4, horizontalCuts = [3,1], verticalCuts = [1]
输出：6
解释：上图所示的矩阵蛋糕中，红色线表示水平和竖直方向上的切口。切割蛋糕后，绿色和黄色的两份蛋糕面积最大。
```
示例 3：
```
输入：h = 5, w = 4, horizontalCuts = [3], verticalCuts = [3]
输出：9
```

提示：
- 2 <= h, w <= 10^9
- 1 <= horizontalCuts.length < min(h, 10^5)
- 1 <= verticalCuts.length < min(w, 10^5)
- 1 <= horizontalCuts[i] < h
- 1 <= verticalCuts[i] < w
- 题目数据保证 horizontalCuts 中的所有元素各不相同
- 题目数据保证 verticalCuts 中的所有元素各不相同

### 思路
就是分别找到水平方向和垂直方向切割线之间的最大间隔。这里有一些细节需要注意。

1. 因为数组不是为有序的，需要先对其进行排序；
2. 在遍历完整个数组时，要记得对边界也进行判断。即第一条切割线离 0 和最后一条切割线离最后边界的距离。
3. 对于返回的数，不要忘记对 10 ^ 9 + 7 取模

### 代码
```c++
class Solution {
public:
    int maxArea(int h, int w, vector<int>& horizontalCuts, vector<int>& verticalCuts) {
        long res = 0;
        int n = horizontalCuts.size();
        int m = verticalCuts.size();
        sort(horizontalCuts.begin(),horizontalCuts.end());
        sort(verticalCuts.begin(),verticalCuts.end());
        long maxw = verticalCuts[0]-0;
        long maxh = horizontalCuts[0]-0;
        for (int i=1;i<n;i++)
        {
            maxh = max(maxh,(long)(horizontalCuts[i]-horizontalCuts[i-1]));  // max是要比较两种同类型的，后边不加(long)会报错,因为默认是int类型
        }
        maxh = max(maxh, (long)(h-horizontalCuts[n-1]));
        for (int i=1;i<m;i++)
        {
            maxw = max(maxw,(long)(verticalCuts[i]-verticalCuts[i-1]));
        }
        maxw = max(maxw, (long)(w-verticalCuts[m-1]));
        res = (maxw*maxh)%(1000000007);
        // 这里如果要使用pow(10,9)+7要 加一个(int)
        // 因为pow返回的是一个double值需要进行一个类型转换
        // 最方便的是 最开始加一个#define N 1000000007
        return res;
    }
};
```
简短点的写法
```c++
class Solution {
public:
    int maxArea(int h, int w, vector<int>& hs, vector<int>& vs) {
        hs.push_back(0); hs.push_back(h); 
        vs.push_back(0); vs.push_back(w);
        
        int x = 0; int y = 0;
        for (int i= 1; i < hs.size(); i++) x = max(x,hs[i] - hs[i-1]);
        for (int i= 0; i < vs.size(); i++) y = max(y,vs[i]-vs[i-1]);

        long long res = x * y % 1000000007;
        return res;
    }
};

```

### [5426. 重新规划路线](https://leetcode-cn.com/problems/pseudo-palindromic-paths-in-a-binary-tree/)

n 座城市，从 0 到 n-1 编号，其间共有 n-1 条路线。因此，要想在两座不同城市之间旅行只有唯一一条路线可供选择（路线网形成一颗树）。去年，交通运输部决定重新规划路线，以改变交通拥堵的状况。

路线用 connections 表示，其中 connections[i] = [a, b] 表示从城市 a 到 b 的一条有向路线。

今年，城市 0 将会举办一场大型比赛，很多游客都想前往城市 0 。

请你帮助重新规划路线方向，使每个城市都可以访问城市 0 。返回需要变更方向的最小路线数。

题目数据 保证 每个城市在重新规划路线方向后都能到达城市 0 。

示例 1：
![](3.png)
```
输入：n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
输出：3
解释：更改以红色显示的路线的方向，使每个城市都可以到达城市 0 。
```

示例 2：
![](4.png)
```
输入：n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
输出：2
解释：更改以红色显示的路线的方向，使每个城市都可以到达城市 0 。
```
示例 3：
```
输入：n = 3, connections = [[1,0],[2,0]]
输出：0
```

提示：
- 2 <= n <= 5 * 10^4
- connections.length == n-1
- connections[i].length == 2
- 0 <= connections[i][0], connections[i][1] <= n-1
- connections[i][0] != connections[i][1]

### 思路

这个题卡住了，没有很好的思路，还是不够熟练，树、图的题还是要多做一些，多多练习。

参考了一下题解，要理解这个题的知识点是什么。

就是树的遍历，就是以0为根节点，遍历该树，如果边不是从上到下就翻转+1。

![](5.png)

帮助理解下边dfs代码：

![](6.jpeg)

### 代码
dfs
```c++
class Solution {
public:
    vector<vector<pair<int,int>>> g;  // 存边号也要存一个方向
    int minReorder(int n, vector<vector<int>>& connections) {
        g = vector<vector<pair<int,int>>>(n); // 初始化为无向边
        for (auto e : connections)
        {
            int a = e[0], b = e[1];
            g[a].push_back({b,1}); // 后边数字表示边的方向，1表示真实的方向,就是与原方向一致
            g[b].push_back({a,0}); // 0表示反向
        }
        return dfs(0,-1);  // -1 表示farther节点，标明是从哪个节点过来的，防止重复遍历
    }

    int dfs (int u, int father){
        int res = 0;
        for (auto e : g[u]){  // 遍历u的所有边
            int ver = e.first, dir = e.second;
            if (ver == father) continue; // 如果节点编号是来的这个点，就是说是从farther下来的
            res += dfs(ver, u) + dir;
        }
        return res;
    }
};
```

利用set来求解
```c++
/*
思路：将可以到达0节点的节点放入v1。初始化v1里面放0节点即v1 = [0]，然后开始遍历，以 [[0,1],[1,3],[2,3],[4,0],[4,5]]为例。
首先是[0,1],目的节点1不在v1中，需要翻转，res =1，然后将1加入v1，v1=[0,1]
遍历到[1,3],目的节点3不在v1中，需要翻转，res = 2，然后将3加入v1，v1=[0,1,3]
遍历到[2,3],目的节点3在v1中，不需要翻转，同时证明2节点可以到达0节点，所以将2加入v1，v1=[0,1,2,3]
遍历到[4,0],目的节点0在v1中，不需要翻转，同时证明4节点可以到达0节点，所以将4加入v1，v1=[0,1,2,3,4]
遍历到[4,5],目的节点5不在v1中，需要翻转，res = 3,将节点5加入v1，v1=[0,1,2,3,4,5]
*/

class Solution {
public:
    int minReorder(int n, vector<vector<int>>& connections) {
        int res = 0;
        set<int> v;
        v.insert(0); // v保存已经能正确到达0的城市
        for (int i=0;i<connections.size();i++)
        {
            if(v.count(connections[i][1])){ //如果该路线终点是已经能到0的，那么指向该终点也能到0
                v.insert(connections[i][0]);
            }
            else{ // 否则 需要翻转res++,然后修改之后添加进v
                res++;  
                v.insert(connections[i][1]);
            }
        }
        return res;
    }
};
```

### [5427. 两个盒子中球的颜色数相同的概率](https://leetcode-cn.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/)

桌面上有 2n 个颜色不完全相同的球，球上的颜色共有 k 种。给你一个大小为 k 的整数数组 balls ，其中 balls[i] 是颜色为 i 的球的数量。

所有的球都已经 随机打乱顺序 ，前 n 个球放入第一个盒子，后 n 个球放入另一个盒子（请认真阅读示例 2 的解释部分）。

注意：这两个盒子是不同的。例如，两个球颜色分别为 a 和 b，盒子分别为 [] 和 ()，那么 [a] (b) 和 [b] (a) 这两种分配方式是不同的（请认真阅读示例 1 的解释部分）。

请计算「两个盒子中球的颜色数相同」的情况的概率。

示例 1：

```
输入：balls = [1,1]
输出：1.00000
解释：球平均分配的方式只有两种：
- 颜色为 1 的球放入第一个盒子，颜色为 2 的球放入第二个盒子
- 颜色为 2 的球放入第一个盒子，颜色为 1 的球放入第二个盒子
这两种分配，两个盒子中球的颜色数都相同。所以概率为 2/2 = 1 。
```

示例 2：

```
输入：balls = [2,1,1]
输出：0.66667
解释：球的列表为 [1, 1, 2, 3]
随机打乱，得到 12 种等概率的不同打乱方案，每种方案概率为 1/12 ：
[1,1 / 2,3], [1,1 / 3,2], [1,2 / 1,3], [1,2 / 3,1], [1,3 / 1,2], [1,3 / 2,1], [2,1 / 1,3], [2,1 / 3,1], [2,3 / 1,1], [3,1 / 1,2], [3,1 / 2,1], [3,2 / 1,1]
然后，我们将前两个球放入第一个盒子，后两个球放入第二个盒子。
这 12 种可能的随机打乱方式中的 8 种满足「两个盒子中球的颜色数相同」。
概率 = 8/12 = 0.66667
```
示例 3：
```
输入：balls = [1,2,1,2]
输出：0.60000
解释：球的列表为 [1, 2, 2, 3, 4, 4]。要想显示所有 180 种随机打乱方案是很难的，但只检查「两个盒子中球的颜色数相同」的 108 种情况是比较容易的。
概率 = 108 / 180 = 0.6 。
```
示例 4：
```
输入：balls = [3,2,1]
输出：0.30000
解释：球的列表为 [1, 1, 1, 2, 2, 3]。要想显示所有 60 种随机打乱方案是很难的，但只检查「两个盒子中球的颜色数相同」的 18 种情况是比较容易的。
概率 = 18 / 60 = 0.3 。
```
示例 5：
```
输入：balls = [6,6,6,6,6,6]
输出：0.90327
```

提示：
- 1 <= balls.length <= 8
- 1 <= balls[i] <= 6
- sum(balls) 是偶数
- 答案与真实值误差在 10^-5 以内，则被视为正确答案

### 思路

比赛时没有看这道题。有点难，用到数学公式，有重复元素的全排列公式如下：

。

具体参考[题解](https://www.bilibili.com/video/BV1PA411q7c5/),[C++解法](https://leetcode-cn.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/solution/dfsjian-zhi-xiang-jie-by-wangdh15/)

### 代码

```c++
class Solution:
    
    def fact(self, n):
        res = 1
        for i in range(1, n + 1): res *= i
        return res
    
    def get_tot(self, balls):
        tot = self.fact(sum(balls))
        for b in balls:
            if b:
                tot /= self.fact(b)
        return tot
    
    def dfs(self, u, balls, left, right, ts, ls, rs):
        if ls * 2 > ts or rs * 2 > ts: return 0
        if u == len(balls):
            l = 0
            r = 0
            for i in range(len(balls)):
                if left[i]: l += 1
                if right[i]: r += 1
            if l != r: return 0
            return self.get_tot(left) * self.get_tot(right)
        res = 0
        for i in range(balls[u] + 1):
            left[u] = i
            right[u] = balls[u] - i
            res += self.dfs(u + 1, balls, left, right, ts, ls + left[u], rs + right[u])
        return res
    
    def getProbability(self, balls: List[int]) -> float:
        tot = self.get_tot(balls)
        left = [0 for i in range(len(balls))]
        right = [0 for i in range(len(balls))]
        return self.dfs(0, balls, left, right, sum(balls), 0, 0) / tot
```