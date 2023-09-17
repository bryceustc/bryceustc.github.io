---
title: 【Leetcode】第217场周赛
date: 2020-11-29 11:58:15
tags: LeetCode
categories: LeetCode
mathjax: true
---
鸽了两周，这次参加周赛有点手生，这次周赛难度也有点大，也是最近也是忙着赶小论文，小论文也准备投出去了，工作也基本定下来了，真的运气是比较好，自己还有很大提升的空间，多撸一撸代码，多点自己的思考，也多看些书和课程提升一下自己的硬实力，冲冲冲！同时也打算开个个人微信公众号记录一下自己的学习生活。Adventure is out there. 做一个终身学习者！加油！
### [5613. 最富有客户的资产总量](https://leetcode-cn.com/problems/richest-customer-wealth/)

给你一个 m x n 的整数网格 accounts ，其中 accounts[i][j] 是第 i​​​​​​​​​​​​ 位客户在第 j 家银行托管的资产数量。返回最富有客户所拥有的 资产总量 。

客户的 资产总量 就是他们在各家银行托管的资产数量之和。最富有客户就是 资产总量 最大的客户。

<!--more-->
示例1：
```
输入：accounts = [[1,2,3],[3,2,1]]
输出：6
解释：
第 1 位客户的资产总量 = 1 + 2 + 3 = 6
第 2 位客户的资产总量 = 3 + 2 + 1 = 6
两位客户都是最富有的，资产总量都是 6 ，所以返回 6 。
```

示例2:
```
输入：accounts = [[1,5],[7,3],[3,5]]
输出：10
解释：
第 1 位客户的资产总量 = 6
第 2 位客户的资产总量 = 10 
第 3 位客户的资产总量 = 8
第 2 位客户是最富有的，资产总量是 10
```

示例3：
```
输入：accounts = [[2,8,7],[7,1,3],[1,9,5]]
输出：17
```

提示：
- m == accounts.length
- n == accounts[i].length
- 1 <= m, n <= 50
- 1 <= accounts[i][j] <= 100

### 思路

暴力模拟

### 代码
```c++
class Solution {
public:
    int maximumWealth(vector<vector<int>>& accounts) {
        int res = 0;
        int m = accounts.size();
        int n = accounts[0].size();
        for (int i = 0; i < m; i++) {
            int sum = 0;
            for (int j = 0; j < n; j++) {
                sum += accounts[i][j];
            }
            res = max(res, sum);
        }
        return res;
    }
};
```

### [5614. 找出最具竞争力的子序列](https://leetcode-cn.com/problems/find-the-most-competitive-subsequence/)

给你一个整数数组 nums 和一个正整数 k ，返回长度为 k 且最具 竞争力 的 nums 子序列。

数组的子序列是从数组中删除一些元素（可能不删除元素）得到的序列。

在子序列 a 和子序列 b 第一个不相同的位置上，如果 a 中的数字小于 b 中对应的数字，那么我们称子序列 a 比子序列 b（相同长度下）更具 竞争力 。 例如，[1,3,4] 比 [1,3,5] 更具竞争力，在第一个不相同的位置，也就是最后一个位置上， 4 小于 5 。

示例 1：
```
输入：nums = [3,5,2,6], k = 2
输出：[2,6]
解释：在所有可能的子序列集合 {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]} 中，[2,6] 最具竞争力。
```

示例 2：
```
输入：nums = [2,4,3,3,5,4,9,6], k = 4
输出：[2,3,3,4]
```

提示：
- 1 <= nums.length <= 105
- 0 <= nums[i] <= 109
- 1 <= k <= nums.length

### 思路
此题与[移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)有点类似

此题后知后觉了，一开始想先dfs枚举出所有符合要求的子序列，然后再从里边找出合适的子序列。但这样时间复杂很高，深搜的话是n！的复杂度，肯定不可以。

子序列问题考虑到单调栈的使用。
### 代码
单调栈
```c++
class Solution {
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) {
        int n = nums.size();
        if (n == k) return nums;
        stack<int> s;
        vector<int> res (k);
        for (int i = 0; i < n; i++) {
            while(!s.empty() && k < n - i + s.size()  && nums[i] < s.top()) {
                s.pop();
            }
            if (s.size() < k) {
                s.push(nums[i]);
            }
        }
        while (!s.empty()) {
            int t = s.top();
            s.pop();
            res[--k] = t;
        }
        return res;
    }
};
```

### [1674. 使数组互补的最少操作次数](https://leetcode-cn.com/problems/minimum-moves-to-make-array-complementary/)

给你一个长度为 偶数 n 的整数数组 nums 和一个整数 limit 。每一次操作，你可以将 nums 中的任何整数替换为 1 到 limit 之间的另一个整数。

如果对于所有下标 i（下标从 0 开始），nums[i] + nums[n - 1 - i] 都等于同一个数，则数组 nums 是 互补的 。例如，数组 [1,2,3,4] 是互补的，因为对于所有下标 i ，nums[i] + nums[n - 1 - i] = 5 。

返回使数组 互补 的 最少 操作次数。

示例 1：
```
输入：nums = [1,2,4,3], limit = 4
输出：1
解释：经过 1 次操作，你可以将数组 nums 变成 [1,2,2,3]（加粗元素是变更的数字）：
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
对于每个 i ，nums[i] + nums[n-1-i] = 4 ，所以 nums 是互补的。
```

示例 2：
```
输入：nums = [1,2,2,1], limit = 2
输出：2
解释：经过 2 次操作，你可以将数组 nums 变成 [2,2,2,2] 。你不能将任何数字变更为 3 ，因为 3 > limit 。
```

示例 3:
```
输入：nums = [1,2,1,2], limit = 2
输出：0
解释：nums 已经是互补的。
```

提示：
- n == nums.length
- 2 <= n <= 105
- 1 <= nums[i] <= limit <= 105
- n 是偶数。

### 思路

首先我们知道肯定是将数组中所有互补的元素的和最终都变为一个确定的值t.这是建设互补的元素为(a,b),且满足(a≤b),则有以下四种情况：
- a≥t: 此时我们需要将a,b都变为比t小的数，则此时需要替换的步数为2.
b<(t−limit) : 则此时我们需要将a,b都需要变换才能保证a + b = t,则此时需要变换的次数为2.
- a+b=t: 此时不需要变换，则变换步数为0.
- a<t,b≥(t−limit),a+b≠t: 则此时我们可以任意变换a或者b均可满足要求，则需要变换一次。
知道以上情况后，我们只需要枚举所有的目标值t,t可能的取值范围为[2,2limit]，枚举所有的t然后求出最小的变换步数即可，当然如果直接求的话，时间复杂度为O(2limit*n),显然如果直接求的话，肯定会超时的，必须要转换思路。

差分数组,具体思路详见代码
### 代码

```c++
class Solution {
public:
    int minMoves(vector<int>& nums, int limit) {
        // res[x] 表示和是x的时候，需要的调整次数，有效范围是[2, limit * 2]
        vector<int> res(limit * 2+1);

        // 这里用一个差分数组来算出res，有点像求重叠区间最大值的思路
        vector<int> diff(limit * 2+2);

        // 对于每对数字来说，在有的范围内的x，需要一次调整，有的需要两次调整
        for (int i = 0, j = nums.size()-1; i < j; i++, j--) {
            int a = nums[i], b = nums[j];
            if (a > b) {
                swap(a, b);
            }
            // a <= b
            // [2, a]                   对a,b来说，在[2,a]范围内的x都需要进行 2 次变更操作，后面类似
            diff[2] += 2; diff[a+1] -= 2;
            // [a+1, a+b-1]             需要进行 1 次变更
            diff[a+1] += 1; diff[a+b] -= 1;
            // a + b，已经满足，不用变更
            // [a+b+1, b+limit]         需要进行 1 次变更
            diff[a+b+1] += 1; diff[b+limit+1] -= 1;
            // [b+limit+1, 2 * limit]   需要进行 2 次变更，x不会超过 2*limit，所以不用考虑 diff[2*limit+1] -= 2;
            diff[b+limit+1] += 2; // diff[2*limit+1] -= 2;
        }

        // 通过差分算出每个x需要调整的次数
        for (int i = 1; i < res.size(); i++) {
            res[i] = res[i-1] + diff[i];
        }

        int ans = INT_MAX;
        for (int i = 2; i <= limit * 2; i++) {
            ans = min(ans, res[i]);
        }
        return ans;
    }
};
```

### [1675. 数组的最小偏移量](https://leetcode-cn.com/problems/minimize-deviation-in-array/)

给你一个由 n 个正整数组成的数组 nums 。

你可以对数组的任意元素执行任意次数的两类操作：

如果元素是 偶数 ，除以 2
例如，如果数组是 [1,2,3,4] ，那么你可以对最后一个元素执行此操作，使其变成 [1,2,3,2]
如果元素是 奇数 ，乘上 2
例如，如果数组是 [1,2,3,4] ，那么你可以对第一个元素执行此操作，使其变成 [2,2,3,4]
数组的 偏移量 是数组中任意两个元素之间的 最大差值 。

返回数组在执行某些操作之后可以拥有的 最小偏移量 。

示例 1：
```
输入：nums = [1,2,3,4]
输出：1
解释：你可以将数组转换为 [1,2,3,2]，然后转换成 [2,2,3,2]，偏移量是 3 - 2 = 1
```

示例 2：
```
输入：nums = [4,1,5,20,3]
输出：3
解释：两次操作后，你可以将数组转换为 [4,2,5,5,3]，偏移量是 5 - 2 = 3
```

示例3：
```
输入：nums = [2,10,8]
输出：3
```
s
提示：
- n == nums.length
- 2 <= n <= 10^5
- 1 <= nums[i] <= 10^9

### 思路
此题是个模板题。

这题的关键是找到数组中小于和大于某个数num的数。此题是要利用树状数组。注意这个知识点

$$a_b$$

### 代码
```c++
#define LL long long
const LL MOD = 1e9 + 7;
const int N = 1e5 + 50;

int sum[N]; // sum[N] 存放前缀和
int lowbit(int x){ return x & -x; }
void add(int x){
    for (int i = x; i <= N; i += lowbit(i)) sum[i] += 1;
}
int get(int x){
    int res = 0;
    for (int i = x; i; i -= lowbit(i)) res += sum[i];
    return res;
}

class Solution {
public:
    int createSortedArray(vector<int>& I) {
        memset(sum, 0, sizeof(sum)); 
        LL res = 0;
        
        int n = I.size();
        for (int i = 0; i < n; i++){
            int x = I[i];
            
            int low = get(x - 1);
            int high = i - get(x);
            res += min(low, high);
            add(x); 
        }
        
        return res % MOD;
    }
};
```