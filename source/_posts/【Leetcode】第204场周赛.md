---
title: 【Leetcode】第204场周赛
date: 2020-08-30 11:46:32
vtags: LeetCode
categories: LeetCode
mathjax: true
---



### [5499. 重复至少 K 次且长度为 M 的模式](https://leetcode-cn.com/problems/detect-pattern-of-length-m-repeated-k-or-more-times/)

给你一个正整数数组 arr，请你找出一个长度为 m 且在数组中至少重复 k 次的模式。

模式 是由一个或多个值组成的子数组（连续的子序列），连续 重复多次但 不重叠 。 模式由其长度和重复次数定义。

如果数组中存在至少重复 k 次且长度为 m 的模式，则返回 true ，否则返回  false 。

<!--more-->
示例1：
```
输入：arr = [1,2,4,4,4,4], m = 1, k = 3
输出：true
解释：模式 (4) 的长度为 1 ，且连续重复 4 次。注意，模式可以重复 k 次或更多次，但不能少于 k 次。
```

示例2:
```
输入：arr = [1,2,1,2,1,1,1,3], m = 2, k = 2
输出：true
解释：模式 (1,2) 长度为 2 ，且连续重复 2 次。另一个符合题意的模式是 (2,1) ，同样重复 2 次。
```

示例3：
```
输入：arr = [1,2,1,2,1,3], m = 2, k = 3
输出：false
解释：模式 (1,2) 长度为 2 ，但是只连续重复 2 次。不存在长度为 2 且至少重复 3 次的模式。
```

示例4：
```
输入：arr = [1,2,3,1,2], m = 2, k = 2
输出：false
解释：模式 (1,2) 出现 2 次但并不连续，所以不能算作连续重复 2 次。
```

示例5：
```
输入：arr = [2,2,2,2], m = 2, k = 3
输出：false
解释：长度为 2 的模式只有 (2,2) ，但是只连续重复 2 次。注意，不能计算重叠的重复次数。
```

提示：
- 2 <= arr.length <= 100
- 1 <= arr[i] <= 100
- 1 <= m <= 100
- 2 <= k <= 100
### 思路
第一题一般直接暴力枚举就可以。

1. 枚举模式的起点
2. 判断接下来的k*m这一部分是否是第一部分的重复

### 代码
```c++
class Solution {
public:
    bool containsPattern(vector<int>& arr, int m, int k) {
        int n = arr.size();
        for (int i = 0; i + k * m <= n; i++) {
            bool flag = true;
            for (int j = i; j < i + k * m; j++) {
                if (arr[j] != arr[i + (j-i) % m]) {
                    flag = false;
                    break;
                }
            }
            if (flag) return true;
        }
        return false;
    }
};
```

### [5500. 乘积为正数的最长子数组长度](https://leetcode-cn.com/problems/maximum-length-of-subarray-with-positive-product/)

给你一个整数数组 nums ，请你求出乘积为正数的最长子数组的长度。

一个数组的子数组是由原数组中零个或者更多个连续数字组成的数组。

请你返回乘积为正数的最长子数组长度。

示例 1：

```
输入：nums = [1,-2,-3,4]
输出：4
解释：数组本身乘积就是正数，值为 24 。
```

示例 2：
```
输入：nums = [0,1,-2,-3,-4]
输出：3
解释：最长乘积为正数的子数组为 [1,-2,-3] ，乘积为 6 。
注意，我们不能把 0 也包括到子数组中，因为这样乘积为 0 ，不是正数。
```

示例 3:
```
输入：nums = [-1,-2,-3,0,1]
输出：2
解释：乘积为正数的最长子数组是 [-1,-2] 或者 [-2,-3] 。
```

示例 4：
```
输入：nums = [1,2,3,5,-6,4,0,10]
输出：4
```

示例 5：
```
输入：nums = [-1,2]
输出：1
```

提示：
- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9

### 思路


### 代码

暴力枚举所有的子数组然后判断，超时
```c++
class Solution {
public:
    int getMaxLen(vector<int>& nums) {
        int n = nums.size();
        int res = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (check(nums, i, j)) {
                    res = max (res, j - i + 1);
                }
            }
        }
        return res;
    }

    bool check(vector<int> &nums, int i, int j) {
        long long res = 1;
        for (int k = i; k < j + 1; k++) {
            res *= nums[k];
        }
        return res > 0;
    }
};
```

dp
```c++
class Solution {
public:
    int getMaxLen(vector<int>& nums) {
        int n = nums.size();
        // f[i] 表示以nums[i]结尾的乘积为正数的最长子数组的长度
        // g[i] 表示以nums[i]结尾的乘积为负数的最长子数组的长度
        vector<int> f(n), g(n);
        if (nums[0] > 0) f[0] = 1;
        else if (nums[0] < 0) g[0] = 1;
        int res = f[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] > 0) {
                f[i] = f[i-1] + 1;
                // 如果g[i-1]为0, g[i]也为0
                if (g[i-1]) g[i] = g[i-1] + 1;
            } else if (nums[i] == 0) {
                f[i] = g[i] = 0;
            } else if (nums[i] < 0) {
                // 如果g[i-1]为0，f[i]也为0
                if (g[i-1]) f[i] = g[i-1] + 1;
                g[i] = f[i-1] + 1;
            }
            res = max (res, f[i]);
        }
        return res;
    }
};
```

### [5489. 两球之间的磁力](https://leetcode-cn.com/problems/magnetic-force-between-two-balls/)

在代号为 C-137 的地球上，Rick 发现如果他将两个球放在他新发明的篮子里，它们之间会形成特殊形式的磁力。Rick 有 n 个空的篮子，第 i 个篮子的位置在 position[i] ，Morty 想把 m 个球放到这些篮子里，使得任意两球间 最小磁力 最大。

已知两个球如果分别位于 x 和 y ，那么它们之间的磁力为 |x - y| 。

给你一个整数数组 position 和一个整数 m ，请你返回最大化的最小磁力。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/16/q3v1.jpg)
```
输入：position = [1,2,3,4,7], m = 3
输出：3
解释：将 3 个球分别放入位于 1，4 和 7 的三个篮子，两球间的磁力分别为 [3, 3, 6]。最小磁力为 3 。我们没办法让最小磁力大于 3 。
```

示例 2：
```
输入：position = [5,4,3,2,1,1000000000], m = 2
输出：999999999
解释：我们使用位于 1 和 1000000000 的篮子时最小磁力最大。
```

提示：
- n == position.length
- 2 <= n <= 10^5
- 1 <= position[i] <= 10^9
- 所有 position 中的整数 互不相同 。
- 2 <= m <= position.length

### 思路

碰到最大化最小值，或者最小化最大值都是利用二分来做。一定要熟练掌握y总给的二分模板！！

- 首先找到二分查找的左右边界
- 然后每次用O(n)的复杂判断是否满足题目要求
- 最终确定二分中的最大或最小目标值。

### 代码

```c++
class Solution {
public:
    int maxDistance(vector<int>&a, int m) {
        int n = a.size();
        sort(a.begin(), a.end());
        int l = 0;
        int r = a[n-1];
        while (l < r) {
            int mid = l + (r -  l + 1) / 2;
            if (check(mid, a, m)) {
                l = mid; //mid是符合的，mid也可以更大，最优解在右半区间
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    bool check(int mid, vector<int> &a, int m) {
        int last = a[0];
        int n = a.size();
        int cnt = 1;  //符合相邻两个点之间的距离>=mid的个数
        for (int i = 1; i < n; i++) {
            if (a[i] - last >= mid) {
                last = a[i];
                cnt++;
            }
        }
        return cnt >= m;
    }
};
```

### [5490. 吃掉 N 个橘子的最少天数](https://leetcode-cn.com/problems/minimum-cost-to-cut-a-stick/)

厨房里总共有 n 个橘子，你决定每一天选择如下方式之一吃这些橘子：

吃掉一个橘子。
如果剩余橘子数 n 能被 2 整除，那么你可以吃掉 n/2 个橘子。
如果剩余橘子数 n 能被 3 整除，那么你可以吃掉 2*(n/3) 个橘子。
每天你只能从以上 3 种方案中选择一种方案。

请你返回吃掉所有 n 个橘子的最少天数。

示例 1：

```
输入：n = 10
输出：4
解释：你总共有 10 个橘子。
第 1 天：吃 1 个橘子，剩余橘子数 10 - 1 = 9。
第 2 天：吃 6 个橘子，剩余橘子数 9 - 2*(9/3) = 9 - 6 = 3。（9 可以被 3 整除）
第 3 天：吃 2 个橘子，剩余橘子数 3 - 2*(3/3) = 3 - 2 = 1。
第 4 天：吃掉最后 1 个橘子，剩余橘子数 1 - 1 = 0。
你需要至少 4 天吃掉 10 个橘子。
```

示例 2：
```
输入：n = 6
输出：3
解释：你总共有 6 个橘子。
第 1 天：吃 3 个橘子，剩余橘子数 6 - 6/2 = 6 - 3 = 3。（6 可以被 2 整除）
第 2 天：吃 2 个橘子，剩余橘子数 3 - 2*(3/3) = 3 - 2 = 1。（3 可以被 3 整除）
第 3 天：吃掉剩余 1 个橘子，剩余橘子数 1 - 1 = 0。
你至少需要 3 天吃掉 6 个橘子。
```

示例3：
```
输入：n = 1
输出：1
```

示例4：
```
输入：n = 56
输出：6
```

提示：
- 1 <= n <= 2*10^9

### 思路



### 代码

比赛超时代码（dp)
```c++
class Solution {
public:
    int minDays(int n) {
        vector<int> dp(n+1, n+1);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            if ((i &1) ==0 && i%3 == 0) {
                dp[i] = min(min(dp[i/2] + 1, dp[i/3]+1), dp[i-1] +1);
            } else if ((i &1) ==0 && i%3 != 0) {
                dp[i] = min(dp[i/2] + 1, dp[i-1]+1);
            } else if ((i &1) !=0 && i%3 == 0) {
                dp[i] = min(dp[i/3] + 1, dp[i-1]+1);
            } else {
                dp[i] = dp[i-1] + 1;
            }
        }
        return dp[n];
    }
};
```

优化的记忆化递归搜索
```c++
class Solution {
public:
    unordered_map<int, int> dp; //记录已经计算完成的n的值
    int minDays(int n) {
        return dfs(n);
    }
    int dfs (int n) {
        if (n == 1) return 1;
        if (n == 2 || n == 3) return 2;

        if (dp.count(n)) return dp[n];
        //最经典的地方， 对 n 整除 2  dfs(n / 2) + n % 2 + 1 ， 对 n 整除 3  dfs(n / 3) + n % 3 + 1
        int res = min(dfs(n/2) + 1 + n % 2, dfs(n / 3) + 1 + n % 3);
        dp[n] = res;
        return dp[n];
    }
};
```

求最短路想到bfs
```c++
class Solution {
public:
    unordered_map<int, int> dp;
    queue<int> q;

    void extend(int dist, int x) {
        // 已经计算过了，直接返回
        if (dp.count(x)) return;
        dp[x] = dist + 1;
        q.push(x);
    }

    int minDays(int n) {
        if (n==0 || n == 1) return n;
        q.push(n);
        while (!q.empty()) {
            int t = q.front();
            q.pop();
            // t == 0表示吃完了 直接返回
            if (t == 0) return dp[t];
            extend(dp[t], t - 1);
            if (t % 3 == 0) extend(dp[t], t/3);
            if (t % 2 == 0) extend(dp[t], t/2);
        }
        return -1;
    }
};
```