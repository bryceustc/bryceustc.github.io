---
title: 【Leetcode】第202场周赛
date: 2020-08-16 15:07:02
vtags: LeetCode
categories: LeetCode
mathjax: true
---

本次周赛ac3道，第四题超时，今天题目比较简单，第一题循环条件出错卡了一下，再认真一点。 加油！冲冲冲！

First make it work, then make it right, and, finally, make it fast.

![](1.png)

### [5185. 存在连续三个奇数的数组](https://leetcode-cn.com/problems/three-consecutive-odds/)

给你一个整数数组 arr，请你判断数组中是否存在连续三个元素都是奇数的情况：如果存在，请返回 true ；否则，返回 false 。
<!--more-->
示例1：
```
输入：arr = [2,6,4,1]
输出：false
解释：不存在连续三个元素都是奇数的情况。
```

示例2:
```
输入：arr = [1,2,34,3,4,5,7,23,12]
输出：true
解释：存在连续三个元素都是奇数的情况，即 [5,7,23] 。
```

提示：
- 1 <= arr.length <= 1000
- 1 <= arr[i] <= 1000
### 思路

直接暴力循环模拟即可，自己比赛时未注意循环条件写错。注意一下。

### 代码
```c++
class Solution {
public:
    bool threeConsecutiveOdds(vector<int>& arr) {
        int n = arr.size();
        for (int i = 0; i < n - 2; i++) {
            if ((arr[i] &1) && (arr[i+1]&1) && (arr[i+2]&1)) return true;
        }
        return false;
    }
};
```

**比赛ac**
```c++
public:
    bool threeConsecutiveOdds(vector<int>& arr) {
        int n = arr.size();
        int left = 0, right = 0;
        int count = 0;
        while (right < n) {
            int a = arr[right++];
            if (count == 3) return true;
            if (a & 1) {
                count ++;
                continue;
            } else {
                count = 0;
            }
        }
        return false;
    }
}
```

### [5488. 使数组中所有元素相等的最小操作数](https://leetcode-cn.com/problems/minimum-operations-to-make-array-equal/)

存在一个长度为 n 的数组 arr ，其中 arr[i] = (2 * i) + 1 （ 0 <= i < n ）。

一次操作中，你可以选出两个下标，记作 x 和 y （ 0 <= x, y < n ）并使 arr[x] 减去 1 、arr[y] 加上 1 （即 arr[x] -=1 且 arr[y] += 1 ）。最终的目标是使数组中的所有元素都 相等 。题目测试用例将会 保证 ：在执行若干步操作后，数组中的所有元素最终可以全部相等。

给你一个整数 n，即数组的长度。请你返回使数组 arr 中所有元素相等所需的 最小操作数 。

示例 1：

```
输入：n = 3
输出：2
解释：arr = [1, 3, 5]
第一次操作选出 x = 2 和 y = 0，使数组变为 [2, 3, 4]
第二次操作继续选出 x = 2 和 y = 0，数组将会变成 [3, 3, 3]
```

示例 2：
```
输入：n = 6
输出：9
```

提示：
- 1 <= n <= 10^4

### 思路
每次操作+1，-1，总和不变，最后各元素相等，一定为平均数，先求出平均数，即可。

### 代码
```c++
/***比赛时提交的代码***/
class Solution {
public:
    int minOperations(int n) {
        int res = 0;
        int mid = 0;
        if (n&1) {
            mid = ((n-1)/2) * 2 +1;
        } else {
            mid = (((n-1)/2) * 2 +1 + ((n)/2) * 2 +1) / 2;
        }
        for (int i = 0; i < n/2; i++) {
            res += mid - (2*i+1);
        }
        return res;
    }
};
```


```c++
class Solution {
public:
    int minOperations(int n) {
        int res = 0;
        for (int i = 0; i < n; i++) {
            res += abs(n - (2*i+1));
        }
        return res / 2;
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

区间dp问题。（奈何我之前不会，多总结多学习）

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