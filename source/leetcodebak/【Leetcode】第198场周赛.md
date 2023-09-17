---
title: 【Leetcode】第198场周赛
date: 2020-07-19 14:40:26
tags: LeetCode
categories: LeetCode
mathjax: true
---
## 第197场周赛

本周题目AC 1/4，第二题ac用例49/57，递归回溯超时，第三题没有思路，感觉是动态规划，第四题模拟超时ac用例11/26.继续贴排名吧，排名很不理想，希望知耻而后勇。只能说自己是个勤奋的人，但不是个聪明的人。希望自己能够坚持下去。
![](1.png)

### [5460. 换酒问题](https://leetcode-cn.com/problems/water-bottles/)

小区便利店正在促销，用 numExchange 个空酒瓶可以兑换一瓶新酒。你购入了 numBottles 瓶酒。

如果喝掉了酒瓶中的酒，那么酒瓶就会变成空的。

请你计算 最多 能喝到多少瓶酒。
<!--more-->
示例1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/sample_1_1875.png)
```
输入：numBottles = 9, numExchange = 3
输出：13
解释：你可以用 3 个空酒瓶兑换 1 瓶酒。
所以最多能喝到 9 + 3 + 1 = 13 瓶酒。
```

示例2:
```
输入：numBottles = 15, numExchange = 4
输出：19
解释：你可以用 4 个空酒瓶兑换 1 瓶酒。
所以最多能喝到 15 + 3 + 1 = 19 瓶酒。
```

示例3：
```
输入：numBottles = 5, numExchange = 5
输出：6
```
示例4：
```
输入：numBottles = 2, numExchange = 3
输出：2
```

提示：
- 1 <= numBottles <= 100
- 2 <= numExchange <= 100
### 思路

直接模拟

### 代码
```c++
class Solution {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        int res = numBottles;
        int a = numBottles;
        while (a >= numExchange) {
            res += a / numExchange;
            a = a / numExchange + a % numExchange;
        }
        return res;
    }
};
```

### [5465. 子树中标签相同的节点数](https://leetcode-cn.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/)

给你一棵树（即，一个连通的无环无向图），这棵树由编号从 0  到 n - 1 的 n 个节点组成，且恰好有 n - 1 条 edges 。树的根节点为节点 0 ，树上的每一个节点都有一个标签，也就是字符串 labels 中的一个小写字符（编号为 i 的 节点的标签就是 labels[i] ）

边数组 edges 以 edges[i] = [ai, bi] 的形式给出，该格式表示节点 ai 和 bi 之间存在一条边。

返回一个大小为 n 的数组，其中 ans[i] 表示第 i 个节点的子树中与节点 i 标签相同的节点数。

树 T 中的子树是由 T 中的某个节点及其所有后代节点组成的树。


示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e1.jpg)
```
输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], labels = "abaedcd"
输出：[2,1,1,1,1,1,1]
解释：节点 0 的标签为 'a' ，以 'a' 为根节点的子树中，节点 2 的标签也是 'a' ，因此答案为 2 。注意树中的每个节点都是这棵子树的一部分。
节点 1 的标签为 'b' ，节点 1 的子树包含节点 1、4 和 5，但是节点 4、5 的标签与节点 1 不同，故而答案为 1（即，该节点本身）。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e2.jpg)
```
输入：n = 4, edges = [[0,1],[1,2],[0,3]], labels = "bbbb"
输出：[4,2,1,1]
解释：节点 2 的子树中只有节点 2 ，所以答案为 1 。
节点 3 的子树中只有节点 3 ，所以答案为 1 。
节点 1 的子树中包含节点 1 和 2 ，标签都是 'b' ，因此答案为 2 。
节点 0 的子树中包含节点 0、1、2 和 3，标签都是 'b'，因此答案为 4 。
```

示例3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e3.jpg)
```
输入：n = 5, edges = [[0,1],[0,2],[1,3],[0,4]], labels = "aabab"
输出：[3,2,1,1,1]
```

示例4：
```
输入：n = 7, edges = [[0,1],[1,2],[2,3],[3,4],[4,5],[5,6]], labels = "aaabaaa"
输出：[6,5,4,1,3,2,1]
```

提示：
- 1 <= n <= 10^5
- edges.length == n - 1
- edges[i].length == 2
- 0 <= ai, bi < n
- ai != bi
- labels.length == n
- labels 仅由小写英文字母组成

### 思路
树上遍历的同时统计子树内所有标签的出现次数
1. 把边数组 edges 转成邻接表形式
2. dfs 后序遍历

### 代码
```c++
const int maxn = 1e5 + 50;
int cnt[maxn][30];  // cnt[i][j] 表示以 i 为根的子树内，标签为 j 的节点数
class Solution {
public:
    vector<vector<int>> g;
    vector<int> res;
    vector<int> countSubTrees(int n, vector<vector<int>>& edges, string labels) {
        g = vector<vector<int>>(n);
        res = vector<int>(n);
        memset(cnt,0,sizeof(cnt));
        for (auto &v : edges) {
            // 无向边
            g[v[0]].push_back(v[1]);
            g[v[1]].push_back(v[0]);
        }
        dfs(0,-1,labels);
        return res;
    }
    void dfs (int cur,int fat, string &labels) {
        cnt[cur][labels[cur]-'a']++;
        for (auto x : g[cur]) {
            if (x == fat) continue; // 每个节点只能统计自己孩子的信息，不能统计父节点的信息
            dfs (x,cur,labels);
            for (int i = 0; i < 26; ++i) {
                cnt[cur][i] += cnt[x][i];
            }
        }
        res[cur] = cnt[cur][labels[cur]-'a'];
    }
};
```

### [5467. 找到最接近目标值的函数值](https://leetcode-cn.com/problems/find-a-value-of-a-mysterious-function-closest-to-target/)

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/change.png)
Winston 构造了一个如上所示的函数 func 。他有一个整数数组 arr 和一个整数 target ，他想找到让 |func(arr, l, r) - target| 最小的 l 和 r 。

请你返回 |func(arr, l, r) - target| 的最小值。

请注意， func 的输入参数 l 和 r 需要满足 0 <= l, r < arr.length 。

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex1.png)

```
输入：arr = [9,12,3,7,15], target = 5
输出：2
解释：所有可能的 [l,r] 数对包括 [[0,0],[1,1],[2,2],[3,3],[4,4],[0,1],[1,2],[2,3],[3,4],[0,2],[1,3],[2,4],[0,3],[1,4],[0,4]]， Winston 得到的相应结果为 [9,12,3,7,15,8,0,3,7,0,0,3,0,0,0] 。最接近 5 的值是 7 和 3，所以最小差值为 2 。
```

示例 2：

```
输入：arr = [1000000,1000000,1000000], target = 1
输出：999999
解释：Winston 输入函数的所有可能 [l,r] 数对得到的函数值都为 1000000 ，所以最小差值为 999999 。
```
示例 3：
```
输入：arr = [1,2,4,8,16], target = 0
输出：0
```

提示：
- 1 <= arr.length <= 10^5
- 1 <= arr[i] <= 10^6
- 0 <= target <= 10^7

### 思路

比赛思路是暴力枚举所有可能的情况,然后超时了

正经思路：

题目给定的函数 func(arr, l, r) 实际上求的是 arr[l] 到 arr[r] 的「按位与之和」，也就是 arr[l] & arr[l+1] & ... & arr[r-1] & arr[r]

按位与之和有一些非常有趣的性质：
![](2.png)

时间复杂度：$O(n \log C)$，这里的 C 是数组元素的最大范围，本题中 $\log C = \log_2 10^6 \approx 20$
### 代码
```c++
class Solution {
public:
    int closestToTarget(vector<int>& arr, int target) {
        int res = INT_MAX;
        unordered_set<int> s;
        for (int &num : arr) {
            unordered_set<int> new_set;
            s.insert(num);
            res = min(res, abs(num - target));
            // s的长度最大为20
            for (int tmp : s) {
                res = min (res, abs(tmp - target));
                new_set.insert(tmp & num);
            }
            s = new_set;
        }
        return res;
    }
};
```