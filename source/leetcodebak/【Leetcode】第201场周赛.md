---
title: 【Leetcode】第201场周赛
date: 2020-08-09 15:28:36
tags: LeetCode
categories: LeetCode
mathjax: true
---
本次周赛ac2道，第三题7个测试用例超时，前缀和的思想还是不够熟练，在数组中找到某一连续区间和为target的，首先要想到用前缀和，第三题是想到了思路，但是超时，引用KentBeck的话，慢慢来不要想太多，继续加油呀！

First make it work, then make it right, and, finally, make it fast.

### [5483. 整理字符串](https://leetcode-cn.com/problems/count-good-triplets/)

给你一个由大小写英文字母组成的字符串 s 。

一个整理好的字符串中，两个相邻字符 s[i] 和 s[i + 1] 不会同时满足下述条件：

- 0 <= i <= s.length - 2
- s[i] 是小写字符，但 s[i + 1] 是相同的大写字符；反之亦然 。

请你将字符串整理好，每次你都可以从字符串中选出满足上述条件的 两个相邻 字符并删除，直到字符串整理好为止。

请返回整理好的 字符串 。题目保证在给出的约束条件下，测试样例对应的答案是唯一的。

注意：空字符串也属于整理好的字符串，尽管其中没有任何字符。
<!--more-->
示例1：
```
输入：s = "leEeetcode"
输出："leetcode"
解释：无论你第一次选的是 i = 1 还是 i = 2，都会使 "leEeetcode" 缩减为 "leetcode" 。
```

示例2:
```
输入：s = "abBAcC"
输出：""
解释：存在多种不同情况，但所有的情况都会导致相同的结果。例如：
"abBAcC" --> "aAcC" --> "cC" --> ""
"abBAcC" --> "abBA" --> "aA" --> ""
```

示例3：
```
输入：s = "s"
输出："s"
```

提示：
- 1 <= s.length <= 100
- s 只包含小写和大写英文字母
### 思路

直接暴力模拟，直接用一个while循环模拟，不断模拟。

还可以用栈

### 代码
```c++
class Solution {
public:
    string makeGood(string s) {
        string res;
        if (s.empty()) return res;
        while (!s.empty()) {
            int n = s.size();
            for (int i =0; i < n-1; ++i) {
                if (abs(s[i] - s[i+1])==32) {
                    s.erase(i, 2);
                }
            }
            int m = s.size();
            if (m == n ) break;
        }
        res = s;
        return res;
    }
};
```

**栈**
```c++
class Solution {
public:
    string makeGood(string s) {
        string res;
        if (s.empty()) return res;
        stack<char> stk;
        //遍历s
        for (char &c : s) {
            //若栈为空，则直接压栈即可
            if (stk.empty()) {
                stk.push(c);
            } else {
                //栈顶元素
                char t = stk.top();
                //如果当前字母是栈顶元素对应的大写或小写，则弹出栈顶元素
                if (t - 'a' == c - 'A' || t - 'A' == c - 'a') {
                    stk.pop();
                    continue;
                } else {
                    stk.push(c);
                }
            }
        }
        //将栈中元素依次弹出反转即可
        while (!stk.empty()) {
            char c = stk.top();
            res += c;
            stk.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### [5484. 找出第 N 个二进制字符串中的第 K 位](https://leetcode-cn.com/problems/find-kth-bit-in-nth-binary-string/)

给你两个正整数 n 和 k，二进制字符串  Sn 的形成规则如下：

- S1 = "0"
- 当 i > 1 时，Si = Si-1 + "1" + reverse(invert(Si-1))

其中 + 表示串联操作，reverse(x) 返回反转 x 后得到的字符串，而 invert(x) 则会翻转 x 中的每一位（0 变为 1，而 1 变为 0）

例如，符合上述描述的序列的前 4 个字符串依次是：

- S1 = "0"
- S2 = "011"
- S3 = "0111001"
- S4 = "011100110110001"

请你返回  Sn 的 第 k 位字符 ，题目数据保证 k 一定在 Sn 长度范围以内。


示例 1：

```
输入：n = 3, k = 1
输出："0"
解释：S3 为 "0111001"，其第 1 位为 "0" 。
```

示例 2：
```
输入：n = 4, k = 11
输出："1"
解释：S4 为 "011100110110001"，其第 11 位为 "1" 。
```

示例3：
```
输入：n = 1, k = 1
输出："0"
```

示例4：
```
输入：n = 2, k = 3
输出："1"
```

提示：
- 1 <= n <= 20
- 1 <= k <= 2n - 1

### 思路
此题类似数学归纳法，我采用是直接模拟法，不太高效。

正确的解法：
- 对于si他的长度是2^i-1即(1<<i)-1;
- 以下标1开始，那么mid中间的'1'的下标就是length/2+1
- 倘若k就是mid那么就是直接'1'
- 倘若k<mid，表明是在si-1中求第k个字符，因为mid以及mid右边的字符都没有任何意义，可以递归
- 倘若k>mid，右半部分是由si-1先按位取反，再翻转得到的，所以要通过si求si-1则需要反过来操作
- 先翻转，再按位取反

### 代码
```c++
/***比赛时提交的代码***/
class Solution {
public:
    char findKthBit(int n, int k) {
        char c;
        string s1 = "0";
        string res = s1;
        string x;
        for (int i = 1; i <n; ++i) {
            x = res;
            // cout << "x:"  << x << endl;
            string a = invert(x);
            reverse(a.begin(),a.end());  
            // cout << "a:" << a << endl;
            res = x + "1" + a;
        }
        // cout << res << endl;
        c = res[k-1];
        return c;
    }
    
    string invert(string x) {
        string a;
        for (char &c : x) {
            if (c ==  '0'){
                a+="1";
            } else {
                a+="0";
            }
        }
        return a;
    }
};
```


```c++
/*
* 对于si他的长度是2^i-1即(1<<i)-1;
* 以下标1开始，那么mid中间的'1'的下标就是length/2+1
* 倘若k就是mid那么就是直接'1'
* 倘若k<mid，表明是在si-1中求第k个字符，因为mid以及mid右边的字符都没有任何意义，可以递归
* 倘若k>mid，右半部分是由si-1先按位取反，再翻转得到的，所以要通过si求si-1则需要反过来操作
  先翻转，再按位取反
*/
class Solution {
public:
    char findKthBit(int n, int k) {
        int length=(1<<n)-1;
        return dfs(length,k);
    }
    char dfs(int length,int k)
    {
        if (length==1)
            return '0';
        int mid=length/2+1;
        if (mid==k)
            return '1';
        else if (k<mid)
            return dfs(mid-1,k);
        else
        {
            char ans=dfs(mid-1,length-k+1);
            return ans=='1' ? '0' : '1';
        }
        return '-1';
    }
};
```

### [5471. 和为目标值的最大数目不重叠非空子数组数目](https://leetcode-cn.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/)

给你一个数组 nums 和一个整数 target 。

请你返回 非空不重叠 子数组的最大数目，且每个子数组中数字和都为 target 。

示例 1：
```
输入：nums = [1,1,1,1,1], target = 2
输出：2
解释：总共有 2 个不重叠子数组（加粗数字表示） [1,1,1,1,1] ，它们的和为目标值 2 。
```

示例 2：
```
输入：nums = [-1,3,5,1,4,2,-9], target = 6
输出：2
解释：总共有 3 个子数组和为 6 。
([5,1], [4,2], [3,5,1,4,2,-9]) 但只有前 2 个是不重叠的。
```

示例 3：
```
输入：nums = [-2,6,6,3,5,4,1,2,8], target = 10
输出：3
```

示例 4：
```
输入：nums = [0,0,0], target = 0
输出：3
```

提示：
- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- 0 <= target <= 10^6

### 思路

比赛思路先统计所有符合要求的子数组，然后再从子数组中挑出不重叠的，但是统计所有符合要求的子数组中超时了，只通过了62/69，我在这里用到统计和为target的子数组，使用了最笨的原始方法，没有使用**前缀和**的思想，一定要注意这个思想，能够降低时间复杂度。

### 代码

**比赛思路：**
```c++
class Solution {
public:
    int maxNonOverlapping(vector<int>& nums, int target) {
        int res = 0;
        int n = nums.size();
        vector< pair <int, int>> v;
        map<int,int> m;
        int sum = 0;
        m[0] = -1;

        // 比赛直接统计的
        // for (int i =0; i < n; ++i) {
        //     sum = 0;
        //     for (int j = i; j < n; ++j) {
        //         sum += nums[j];
        //         if (sum ==target) {
        //             v.push_back({i, j});
        //         }
        //     }
        // }
        for (int i = 0; i < n; ++i) {
            sum += nums[i];
            if (m.count(sum - target)) {
                v.push_back({m[sum-target]+1, i}); // 注意+1
            }
            m[sum] = i;
        }
        sort (v.begin(), v.end(),cmp);
        int last = -1;
        for (int i = 0; i < v.size(); ++i) {
            // cout << v[i].first << endl;
            // cout << v[i].second<< endl;
            if (v[i].first > last) {
                res++;
                last = v[i].second;
            }
        }
        return res;
    }
    static bool cmp (const pair<int,int> & a, const pair<int,int> & b) {
        return a.second < b.second;
    }
};
```

**O(n)最优解答**
```c++
class Solution {
public:
    int maxNonOverlapping(vector<int>& nums, int target) {
        int res = 0;
        int n = nums.size();
        unordered_map<int,int> m;
        m[0] = -1;
        int sum = 0, last = -1;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            if (m.count(sum-target) && m[sum-target]+1 > last) {
                res++;
                last = i;
            }
            m[sum] = i;
        }
        return res;
    }
};
```

### [5486. 切棍子的最小成本](https://leetcode-cn.com/problems/minimum-cost-to-cut-a-stick/)

有一根长度为 n 个单位的木棍，棍上从 0 到 n 标记了若干位置。例如，长度为 6 的棍子可以标记如下：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/statement.jpg)

给你一个整数数组 cuts ，其中 cuts[i] 表示你需要将棍子切开的位置。

你可以按顺序完成切割，也可以根据需要更改切割的顺序。

每次切割的成本都是当前要切割的棍子的长度，切棍子的总成本是历次切割成本的总和。对棍子进行切割将会把一根木棍分成两根较小的木棍（这两根木棍的长度和就是切割前木棍的长度）。请参阅第一个示例以获得更直观的解释。

返回切棍子的 最小总成本 。

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/e1.jpg)

```
输入：n = 7, cuts = [1,3,4,5]
输出：16
解释：按 [1, 3, 4, 5] 的顺序切割的情况如下所示：

第一次切割长度为 7 的棍子，成本为 7 。第二次切割长度为 6 的棍子（即第一次切割得到的第二根棍子），第三次切割为长度 4 的棍子，最后切割长度为 3 的棍子。总成本为 7 + 6 + 4 + 3 = 20 。
而将切割顺序重新排列为 [3, 5, 1, 4] 后，总成本 = 16（如示例图中 7 + 4 + 3 + 2 = 16）。
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/e11.jpg)

示例 2：
```
输入：n = 9, cuts = [5,6,1,4,2]
输出：22
解释：如果按给定的顺序切割，则总成本为 25 。总成本 <= 25 的切割顺序很多，例如，[4，6，5，2，1] 的总成本 = 22，是所有可能方案中成本最小的。
```

提示：
- 2 <= n <= 10^6
- 1 <= cuts.length <= min(n - 1, 100)
- 1 <= cuts[i] <= n - 1
- cuts 数组中的所有整数都 互不相同

### 思路

区间dp问题。（奈何我之前不会，多总结多学习）

### 代码

```c++
/*
dp[i][j]是i到j之间截断所需最小花费，k是i到j之间所有截断点。
初始化：dp[i][i]==0。相邻截断点之间没有截断点，不需要截断，花费为0。
状态转移方程：dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + cost)
返回答案：dp[1][m] (最终切完有m段)
*/
class Solution {
public:
    int minCost(int n, vector<int>& cuts) {
        vector< vector < int > > dp (105, vector< int > (105, -1));
        cuts.push_back(0);
        cuts.push_back(n);
        // 注意排序，排序之后只需关注cuts位置
        sort( cuts.begin(), cuts.end());
        int m = cuts.size() - 1;  // 切成多少段
        for (int i = 0; i <= m; ++i) {
            dp[i][i] = 0;
        }

        for (int len = 1; len < n; len++) {
            for (int i = 1; i <= m; i++) {
                int j = i + len;
                if (j > m) break;
                for (int k = i; k < j; k++) {
                    if (dp[i][k]==-1 || dp[k+1][j]==-1) continue; // 不可切
                    int cost = dp[i][k] + dp[k+1][j] + cuts[j] - cuts[i-1];
                    // cuts[j] - cuts[i-1] 是指当前木棍的长度
                    // 1 2 3 切2 3 时，木棍长度是3-1=2
                    if (dp[i][j] == -1 || dp[i][j] > v ) {
                        dp[i][j] = cost;
                    }
                }
            }
        }
        return dp[1][m];
    }
};
```