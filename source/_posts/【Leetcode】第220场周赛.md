---
title: 【Leetcode】第220场周赛
date: 2021-01-08 11:01:59
tags: LeetCode
categories: LeetCode
mathjax: true
---

第220场周赛，拖了这么久，补一下。

### [1694. 重新格式化电话号码](https://leetcode-cn.com/problems/reformat-phone-number/)

给你一个字符串形式的电话号码 number 。number 由数字、空格 ' '、和破折号 '-' 组成。

请你按下述方式重新格式化电话号码。

首先，删除 所有的空格和破折号。
其次，将数组从左到右 每 3 个一组 分块，直到 剩下 4 个或更少数字。剩下的数字将按下述规定再分块：
2 个数字：单个含 2 个数字的块。
3 个数字：单个含 3 个数字的块。
4 个数字：两个分别含 2 个数字的块。
最后用破折号将这些块连接起来。注意，重新格式化过程中 不应该 生成仅含 1 个数字的块，并且 最多 生成两个含 2 个数字的块。

返回格式化后的电话号码。

<!--more-->
示例1：
```
输入：number = "1-23-45 6"
输出："123-456"
解释：数字是 "123456"
步骤 1：共有超过 4 个数字，所以先取 3 个数字分为一组。第 1 个块是 "123" 。
步骤 2：剩下 3 个数字，将它们放入单个含 3 个数字的块。第 2 个块是 "456" 。
连接这些块后得到 "123-456" 。
```

示例2:
```
输入：number = "123 4-567"
输出："123-45-67"
解释：数字是 "1234567".
步骤 1：共有超过 4 个数字，所以先取 3 个数字分为一组。第 1 个块是 "123" 。
步骤 2：剩下 4 个数字，所以将它们分成两个含 2 个数字的块。这 2 块分别是 "45" 和 "67" 。
连接这些块后得到 "123-45-67" 。
```

示例3:
```
输入：number = "123 4-5678"
输出："123-456-78"
解释：数字是 "12345678" 。
步骤 1：第 1 个块 "123" 。
步骤 2：第 2 个块 "456" 。
步骤 3：剩下 2 个数字，将它们放入单个含 2 个数字的块。第 3 个块是 "78" 。
连接这些块后得到 "123-456-78" 。
```

示例4：
```
输入：number = "12"
输出："12"
```

示例5：
```
输入：number = "--17-5 229 35-39475 "
输出："175-229-353-94-75"
```

提示：
- 2 <= number.length <= 100
- number 由数字和字符 '-' 及 ' ' 组成。
- number 中至少含 2 个数字。
### 思路
先把出现的数字保存下来，然后逐个分组，模拟法

### 代码
```c++
class Solution {
public:
    string reformatNumber(string number) {
        string res;
        for (char &c : number) {
            if (c != ' ' && c !='-') {
                res += c;
            }
        }
        int n = res.size();
        string ans;
        for (int i = 0; i < n;) {
            if (n - i > 4) {
                ans += res.substr(i, 3);
                ans += '-';
                i+=3;
            } else {
                if (n - i == 4) {
                    ans += res.substr(i, 2);
                    ans += '-';
                    ans += res.substr(i+2, 2);
                    i+= 4;
                } else if (n - i == 3) {
                    ans += res.substr(i, 3);
                    i+=3;
                } else if (n - i == 2) {
                    ans += res.substr(i, 2);
                    i+=2;
                }
            }
        }
        return ans;
    }
};
```

### [1695. 删除子数组的最大得分](https://leetcode-cn.com/problems/maximum-erasure-value/)

给你一个正整数数组 nums ，请你从中删除一个含有 若干不同元素 的子数组。删除子数组的 得分 就是子数组各元素之 和 。

返回 只删除一个 子数组可获得的 最大得分 。

如果数组 b 是数组 a 的一个连续子序列，即如果它等于 a[l],a[l+1],...,a[r] ，那么它就是 a 的一个子数组。

示例 1：

```
输入：nums = [4,2,4,5,6]
输出：17
解释：最优子数组是 [2,4,5,6]    
```

示例 2：
```
输入：nums = [5,2,1,2,5,2,1,2,5]
输出：8
解释：最优子数组是 [5,2,1] 或 [1,2,5]
```

提示：
- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^4

### 思路
滑动窗口模板题，加强熟悉

### 代码
```c++
class Solution {
public:
    int maximumUniqueSubarray(vector<int>& nums) {
        int res = INT_MIN;
        int n = nums.size();
        int left = 0, right = 0;
        int sum = 0;
        unordered_map<int, int> m;
        // 滑动窗口模板
        while (right < n) {
            int a = nums[right++];
            m[a]++;
            sum += a;
            while( left < n && m[a] > 1) {
                m[nums[left]]--;
                sum -= nums[left++];
            }
            res = max(res, sum);
        }
        return res;
    }
};
```

### [1696. 跳跃游戏 VI](https://leetcode-cn.com/problems/jump-game-vi/)

给你一个下标从 0 开始的整数数组 nums 和一个整数 k 。

一开始你在下标 0 处。每一步，你最多可以往前跳 k 步，但你不能跳出数组的边界。也就是说，你可以从下标 i 跳到 [i + 1， min(n - 1, i + k)] 包含 两个端点的任意位置。

你的目标是到达数组最后一个位置（下标为 n - 1 ），你的 得分 为经过的所有数字之和。

请你返回你能得到的 最大得分 。

示例 1：
```
输入：nums = [1,-1,-2,4,-7,3], k = 2
输出：7
解释：你可以选择子序列 [1,-1,4,3] （上面加粗的数字），和为 7 。
```

示例 2：
```
输入：nums = [10,-5,-2,4,0,3], k = 3
输出：17
解释：你可以选择子序列 [10,4,3] （上面加粗数字），和为 17 。
```

示例3：
```
输入：nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
输出：0
```

提示：
- 1 <= nums.length, k <= 10^5
- -10^4 <= nums[i] <= 10^4

### 思路

动态规划 + 大顶堆

### 代码
简单dp，超时
```c++
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> dp(n);
        dp[0] = nums[0];
        for (int i = 1; i < n; i++) {
            int cur = INT_MIN;
            for (int j = max(0, i - k); j < i; j++) {
                cur = max(cur, dp[j]);
            }
            dp[i] = nums[i] + cur;
        }
        return dp[n-1];
    }
};
```

大顶堆优化
```c++
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        
        // 优先队列中的二元组即为 (f[j], j)
        priority_queue<pair<int, int>> q;
        q.push({nums[0], 0});
        int ans = nums[0];
        
        for (int i = 1; i < n; ++i) {
            // 堆顶的 j 不满足限制
            while (i - q.top().second > k) {
                q.pop();
            }
            
            ans = q.top().first + nums[i];
            q.push({ans, i});
        }
        
        return ans;
    }
};
```

### [1697. 检查边长度限制的路径是否存在](https://leetcode-cn.com/problems/checking-existence-of-edge-length-limited-paths/)

给你一个 n 个点组成的无向图边集 edgeList ，其中 edgeList[i] = [ui, vi, disi] 表示点 ui 和点 vi 之间有一条长度为 disi 的边。请注意，两个点之间可能有 超过一条边 。

给你一个查询数组queries ，其中 queries[j] = [pj, qj, limitj] ，你的任务是对于每个查询 queries[j] ，判断是否存在从 pj 到 qj 的路径，且这条路径上的每一条边都 严格小于 limitj 。

请你返回一个 布尔数组 answer ，其中 answer.length == queries.length ，当 queries[j] 的查询结果为 true 时， answer 第 j 个值为 true ，否则为 false 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/19/h.png)
```
输入：n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = [[0,1,2],[0,2,5]]
输出：[false,true]
解释：上图为给定的输入数据。注意到 0 和 1 之间有两条重边，分别为 2 和 16 。
对于第一个查询，0 和 1 之间没有小于 2 的边，所以我们返回 false 。
对于第二个查询，有一条路径（0 -> 1 -> 2）两条边都小于 5 ，所以这个查询我们返回 true 。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/19/q.png)
```
输入：n = 5, edgeList = [[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = [[0,4,14],[1,4,13]]
输出：[true,false]
解释：上图为给定数据。
```

提示：
- 2 <= n <= 105
- 1 <= edgeList.length, queries.length <= 105
- edgeList[i].length == 3
- queries[j].length == 3
- 0 <= ui, vi, pj, qj <= n - 1
- ui != vi
- pj != qj
- 1 <= disi, limitj <= 109
- 两个点之间可能有 多条 边。

### 思路
[并查集 + 离线思维](https://leetcode-cn.com/problems/checking-existence-of-edge-length-limited-paths/solution/jian-cha-bian-chang-du-xian-zhi-de-lu-ji-c756/)


### 代码
```c++
// 并查集模板，包含路径压缩（参考 findset 函数）以及按秩合并（参考 sz 变量）
class UF {
public:
    vector<int> fa;
    vector<int> sz;
    int n;
    int comp_cnt;
    
public:
    UF(int _n): n(_n), comp_cnt(_n), fa(_n), sz(_n, 1) {
        iota(fa.begin(), fa.end(), 0);
    }
    
    int findset(int x) {
        return fa[x] == x ? x : fa[x] = findset(fa[x]);
    }
    
    bool unite(int x, int y) {
        x = findset(x);
        y = findset(y);
        if (x == y) {
            return false;
        }
        if (sz[x] < sz[y]) {
            swap(x, y);
        }
        fa[y] = x;
        sz[x] += sz[y];
        --comp_cnt;
        return true;
    }
    
    bool connected(int x, int y) {
        x = findset(x);
        y = findset(y);
        return x == y;
    }
};

class Solution {
public:
    vector<bool> distanceLimitedPathsExist(int n, vector<vector<int>>& edgeList, vector<vector<int>>& queries) {
        // 将 queries 按照边权限制从小到大排序
        vector<int> qid(queries.size());
        iota(qid.begin(), qid.end(), 0);
        sort(qid.begin(), qid.end(), [&](int i, int j) {
            return queries[i][2] < queries[j][2];
        });
        
        // 将 edgeList 按照边权从小到大排序
        sort(edgeList.begin(), edgeList.end(), [](const auto& e1, const auto& e2) {
            return e1[2] < e2[2];
        });
        
        // 并查集
        UF uf(n);
        
        int i = 0;
        vector<bool> ans(queries.size());
        for (int query: qid) {
            // 往并查集中添加边直到边权关系 dis_i < limit_j 不满足
            while (i < edgeList.size() && edgeList[i][2] < queries[query][2]) {
                uf.unite(edgeList[i][0], edgeList[i][1]);
                ++i;
            }
            // 使用并查集判断连通性
            ans[query] = uf.connected(queries[query][0], queries[query][1]);
        }
        
        return ans;
    }
};
```