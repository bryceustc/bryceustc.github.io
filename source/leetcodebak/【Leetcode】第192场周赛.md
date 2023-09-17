---
title: 【Leetcode】第192场周赛
date: 2020-06-07 17:58:18
tags:
---
## 第192场周赛

本周题目AC 2.2/4，第三题设计题有点可惜，理解题意的逻辑还不够深入，只AC了两道，继续加油，奥利给！


### [5428. 重新排列数组](https://leetcode-cn.com/problems/shuffle-the-array/)

给你一个数组 nums ，数组中有 2n 个元素，按 [x1,x2,...,xn,y1,y2,...,yn] 的格式排列。

请你将数组按 [x1,y1,x2,y2,...,xn,yn] 格式重新排列，返回重排后的数组。

<!--more-->
示例1：
```
输入：nums = [2,5,1,3,4,7], n = 3
输出：[2,3,5,4,1,7] 
解释：由于 x1=2, x2=5, x3=1, y1=3, y2=4, y3=7 ，所以答案为 [2,3,5,4,1,7]
```

示例2:
```
输入：nums = [1,2,3,4,4,3,2,1], n = 4
输出：[1,4,2,3,3,2,4,1]
```

示例3：
```
输入：nums = [1,1,2,2], n = 2
输出：[1,2,1,2]
```
提示：
- 1 <= n <= 500
- nums.length == 2n
- 1 <= nums[i] <= 10^3
### 思路

直接模拟

### 代码
```c++
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        vector<int> res;
        for(int i=0;i<n;i++){
            res.push_back(nums[i]);
            res.push_back(nums[i+n]);
        }
        return res;
    }
};
```

### [5429. 数组中的 k 个最强值](https://leetcode-cn.com/problems/the-k-strongest-values-in-an-array/)

给你一个整数数组 arr 和一个整数 k 。

设 m 为数组的中位数，只要满足下述两个前提之一，就可以判定 arr[i] 的值比 arr[j] 的值更强：
- |arr[i] - m| > |arr[j] - m|
- |arr[i] - m| == |arr[j] - m|，且 arr[i] > arr[j]

请返回由数组中最强的 k 个值组成的列表。答案可以以 任意顺序 返回。


中位数 是一个有序整数列表中处于中间位置的值。形式上，如果列表的长度为 n ，那么中位数就是该有序列表（下标从 0 开始）中位于 ((n - 1) / 2) 的元素。

- 例如 arr = [6, -3, 7, 2, 11]，n = 5：数组排序后得到 arr = [-3, 2, 6, 7, 11] ，数组的中间位置为 m = ((5 - 1) / 2) = 2 ，中位数 arr[m] 的值为 6 。
- 例如 arr = [-7, 22, 17, 3]，n = 4：数组排序后得到 arr = [-7, 3, 17, 22] ，数组的中间位置为 m = ((4 - 1) / 2) = 1 ，中位数 arr[m] 的值为 3 。

示例 1：
```
输入：arr = [1,2,3,4,5], k = 2
输出：[5,1]
解释：中位数为 3，按从强到弱顺序排序后，数组变为 [5,1,4,2,3]。最强的两个元素是 [5, 1]。[1, 5] 也是正确答案。
注意，尽管 |5 - 3| == |1 - 3| ，但是 5 比 1 更强，因为 5 > 1 。
```

示例 2：
```
输入：arr = [1,1,3,5,5], k = 2
输出：[5,5]
解释：中位数为 3, 按从强到弱顺序排序后，数组变为 [5,5,1,1,3]。最强的两个元素是 [5, 5]。
```
示例 3：
```
输入：arr = [6,7,11,7,6,8], k = 5
输出：[11,8,6,6,7]
解释：中位数为 7, 按从强到弱顺序排序后，数组变为 [11,8,6,6,7,7]。
[11,8,6,6,7] 的任何排列都是正确答案。
```
示例4：
```
输入：arr = [6,-3,7,2,11], k = 3
输出：[-3,11,2]
```
示例5：
```
输入：arr = [-7,22,17,3], k = 2
输出：[22,17]
```

提示：
- 1 <= arr.length <= 10^5
- -10^5 <= arr[i] <= 10^5
- 1 <= k <= arr.length

### 思路
1.首先想到的是自定义排序，学习一下lamuda隐式函数的用法

2.双指针

### 代码
自定义排序
```c++
class Solution {
public:
    vector<int> getStrongest(vector<int>& arr, int k) {
        vector<int> res;
        int n = arr.size();
        sort(arr.begin(),arr.end());
        int mid = arr[(n-1)/2];
        vector<pair<int,int>> tmp;
        for (int i=0;i<n;i++){
            tmp.push_back({arr[i]-mid,arr[i]});
        }
        sort(tmp.begin(),tmp.end(),[](const pair<int,int> a, const pair<int,int> b){
            if (abs(a.first) > abs(b.first)){
                return true;
            }
            else if ((abs(a.first) == abs(b.first)) && (a.second > b.second)){
                return true;
            }
            return false;
        });
        for (int i=0;i<k;i++){
            res.push_back(tmp[i].second);
        }
        return res;
    }
};
```
y总的简短写法
```c++
int m;

class Solution {
public:
    static bool cmp(int a, int b) {
        if (abs(a - m) != abs(b - m)) return abs(a - m) > abs(b - m);
        return a > b;
    }
    vector<int> getStrongest(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());
        int n = arr.size();
        m = arr[(n - 1) / 2];
        sort(arr.begin(), arr.end(), cmp);
        vector<int> res(arr.begin(), arr.begin() + k);
        return res;
    }
};
```

双指针
```c++
class Solution {
public:
    vector<int> getStrongest(vector<int>& arr, int k) {
        sort(arr.begin(),arr.end());
        vector<int> res;
        int n = arr.size();
        int l = 0, r = n-1;
        int mid = (n-1)/2;
        for (int i=0;i<k;i++){
            if (arr[mid]-arr[l] > arr[r] - arr[mid] ){
                res.push_back(arr[l]);
                l++;
            }
            else if (arr[mid] - arr[l] == arr[r] - arr[mid]){
                res.push_back(arr[r]);
                r--;
            }
            else{
                res.push_back(arr[r]);
                r--;
            }
        }
        return res;
    }
};
```

### [5430. 设计浏览器历史记录](https://leetcode-cn.com/problems/design-browser-history/)

你有一个只支持单个标签页的 浏览器 ，最开始你浏览的网页是 homepage ，你可以访问其他的网站 url ，也可以在浏览历史中后退 steps 步或前进 steps 步。

请你实现 BrowserHistory 类：

BrowserHistory(string homepage) ，用 homepage 初始化浏览器类。
void visit(string url) 从当前页跳转访问 url 对应的页面  。执行此操作会把浏览历史前进的记录全部删除。
string back(int steps) 在浏览历史中后退 steps 步。如果你只能在浏览历史中后退至多 x 步且 steps > x ，那么你只后退 x 步。请返回后退 至多 steps 步以后的 url 。
string forward(int steps) 在浏览历史中前进 steps 步。如果你只能在浏览历史中前进至多 x 步且 steps > x ，那么你只前进 x 步。请返回前进 至多 steps步以后的 url 。

示例 1：
```
输入：
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
输出：
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

解释：
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // 你原本在浏览 "leetcode.com" 。访问 "google.com"
browserHistory.visit("facebook.com");     // 你原本在浏览 "google.com" 。访问 "facebook.com"
browserHistory.visit("youtube.com");      // 你原本在浏览 "facebook.com" 。访问 "youtube.com"
browserHistory.back(1);                   // 你原本在浏览 "youtube.com" ，后退到 "facebook.com" 并返回 "facebook.com"
browserHistory.back(1);                   // 你原本在浏览 "facebook.com" ，后退到 "google.com" 并返回 "google.com"
browserHistory.forward(1);                // 你原本在浏览 "google.com" ，前进到 "facebook.com" 并返回 "facebook.com"
browserHistory.visit("linkedin.com");     // 你原本在浏览 "facebook.com" 。 访问 "linkedin.com"
browserHistory.forward(2);                // 你原本在浏览 "linkedin.com" ，你无法前进任何步数。
browserHistory.back(2);                   // 你原本在浏览 "linkedin.com" ，后退两步依次先到 "facebook.com" ，然后到 "google.com" ，并返回 "google.com"
browserHistory.back(7);                   // 你原本在浏览 "google.com"， 你只能后退一步到 "leetcode.com" ，并返回 "leetcode.com"
```

提示：
- 1 <= homepage.length <= 20
- 1 <= url.length <= 20
- 1 <= steps <= 100
- homepage 和 url 都只包含 '.' 或者小写英文字母。
- 最多调用 5000 次 visit， back 和 forward 函数。

### 思路

这个题逻辑差一点，visit之后，记得清除当前位置之后的，因为后边再访问就不会再访问历史之前，因为再back只从visit的返回到当前，设计题算法没那么难，关键是认真理清逻辑。

### 代码
```c++
class BrowserHistory {
public:
    BrowserHistory(string homepage) {
        s.push_back(homepage);
        idx=0;
    }
    
    void visit(string url) {
        s.erase(s.begin()+idx+1,s.end());
        s.push_back(url);
        idx = s.size()-1;
    }
    
    string back(int steps) {
        idx = max(0, idx-steps);
        return s[idx];
    }
    
    string forward(int steps) {
        idx = min((int)(s.size()-1), (idx+steps));
        return s[idx];
    }
private:
    vector<string> s;
    int idx;
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```

### [5431. 给房子涂色 III](https://leetcode-cn.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/)

在一个小城市里，有 m 个房子排成一排，你需要给每个房子涂上 n 种颜色之一（颜色编号为 1 到 n ）。有的房子去年夏天已经涂过颜色了，所以这些房子不需要被重新涂色。

我们将连续相同颜色尽可能多的房子称为一个街区。（比方说 houses = [1,2,2,3,3,2,1,1] ，它包含 5 个街区  [{1}, {2,2}, {3,3}, {2}, {1,1}] 。）

注意：这两个盒子是不同的。例如，两个球颜色分别为 a 和 b，盒子分别为 [] 和 ()，那么 [a] (b) 和 [b] (a) 这两种分配方式是不同的（请认真阅读示例 1 的解释部分）。

给你一个数组 houses ，一个 m * n 的矩阵 cost 和一个整数 target ，其中：
- houses[i]：是第 i 个房子的颜色，0 表示这个房子还没有被涂色。
- cost[i][j]：是将第 i 个房子涂成颜色 j+1 的花费。

请你返回房子涂色方案的最小总花费，使得每个房子都被涂色后，恰好组成 target 个街区。如果没有可用的涂色方案，请返回 -1 。

示例 1：

```
输入：houses = [0,0,0,0,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
输出：9
解释：房子涂色方案为 [1,2,2,1,1]
此方案包含 target = 3 个街区，分别是 [{1}, {2,2}, {1,1}]。
涂色的总花费为 (1 + 1 + 1 + 1 + 5) = 9。
```

示例 2：

```
输入：houses = [0,2,1,2,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
输出：11
解释：有的房子已经被涂色了，在此基础上涂色方案为 [2,2,1,2,2]
此方案包含 target = 3 个街区，分别是 [{2,2}, {1}, {2,2}]。
给第一个和最后一个房子涂色的花费为 (10 + 1) = 11。
```
示例 3：
```
输入：houses = [0,0,0,0,0], cost = [[1,10],[10,1],[1,10],[10,1],[1,10]], m = 5, n = 2, target = 5
输出：5
```
示例 4：
```
输入：houses = [3,1,2,3], cost = [[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3
输出：-1
解释：房子已经被涂色并组成了 4 个街区，分别是 [{3},{1},{2},{3}] ，无法形成 target = 3 个街区。
```

提示：
- m == houses.length == cost.length
- n == cost[i].length
- 1 <= m <= 100
- 1 <= n <= 20
- 1 <= target <= m
- 0 <= houses[i] <= n
- 1 <= cost[i][j] <= 10^4

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