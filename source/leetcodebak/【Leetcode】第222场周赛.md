---
title: 【Leetcode】第222场周赛
date: 2021-01-08 11:02:11
tags: LeetCode
categories: LeetCode
mathjax: true
---

第222场周赛

### [1710. 卡车上的最大单元数](https://leetcode-cn.com/problems/maximum-units-on-a-truck/)

请你将一些箱子装在 一辆卡车 上。给你一个二维数组 boxTypes ，其中 boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi] ：

numberOfBoxesi 是类型 i 的箱子的数量。
numberOfUnitsPerBoxi 是类型 i 每个箱子可以装载的单元数量。
整数 truckSize 表示卡车上可以装载 箱子 的 最大数量 。只要箱子数量不超过 truckSize ，你就可以选择任意箱子装到卡车上。

返回卡车可以装载 单元 的 最大 总数。

<!--more-->
示例1：
```
输入：boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
输出：8
解释：箱子的情况如下：
- 1 个第一类的箱子，里面含 3 个单元。
- 2 个第二类的箱子，每个里面含 2 个单元。
- 3 个第三类的箱子，每个里面含 1 个单元。
可以选择第一类和第二类的所有箱子，以及第三类的一个箱子。
单元总数 = (1 * 3) + (2 * 2) + (1 * 1) = 8
```

示例2:
```
输入：boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
输出：91
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
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        int res = 0;
        sort(boxTypes.begin(), boxTypes.end(), cmp);
        int n = boxTypes.size();
        int remain = truckSize;
        for (int i = 0; i < n; i++) {
            if (remain > 0 && remain < boxTypes[i][0]) {
                res += remain * boxTypes[i][1];
                remain = 0;
            } else if (remain > 0 && remain >= boxTypes[i][0]) {
                remain -= boxTypes[i][0];
                res += boxTypes[i][0] * boxTypes[i][1]; 
            }
            if (remain <= 0) break;
        }
        return res;
    }
    static bool cmp(vector<int> &A, vector<int>&B) {
        return A.back() > B.back();
    }
};
```

### [1711. 大餐计数](https://leetcode-cn.com/problems/count-good-meals/)

大餐 是指 恰好包含两道不同餐品 的一餐，其美味程度之和等于 2 的幂。

你可以搭配 任意 两道餐品做一顿大餐。

给你一个整数数组 deliciousness ，其中 deliciousness[i] 是第 i​​​​​​​​​​​​​​ 道餐品的美味程度，返回你可以用数组中的餐品做出的不同 大餐 的数量。结果需要对 109 + 7 取余。

注意，只要餐品下标不同，就可以认为是不同的餐品，即便它们的美味程度相同。

示例 1：

```
输入：deliciousness = [1,3,5,7,9]
输出：4
解释：大餐的美味程度组合为 (1,3) 、(1,7) 、(3,5) 和 (7,9) 。
它们各自的美味程度之和分别为 4 、8 、8 和 16 ，都是 2 的幂。
```

示例 2：
```
输入：deliciousness = [1,1,1,3,3,3,7]
输出：15
解释：大餐的美味程度组合为 3 种 (1,1) ，9 种 (1,3) ，和 3 种 (1,7) 。
```

提示：
- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^4

### 思路
两数之和的变种，注意到此题的输入限制,数字最大为2^20，所以我们可以在有限的空间枚举

### 代码
```c++
class Solution {
public:
    int countPairs(vector<int>& d) {
        const int MOD = 1000000007;
        int n = d.size();
        if(n <= 1) return 0;
        long long res = 0;
        unordered_map<int,int> m;
        for (int &a : d) {
            m[a]++;
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 22; j++) {
                int num = 1 << j;
                if (num - d[i] < 0) continue;
                if (num - d[i] == d[i] && m[d[i]] >= 2) {
                    res += m[d[i]] - 1;
                } else if (num-d[i] != d[i] && m[num-d[i]] > 0){
                    res += m[num - d[i]];
                }
            }
        }
        int ans = (res / 2) % MOD;
        return ans;
    }
};
```

### [1712. 将数组分成三个子数组的方案数](https://leetcode-cn.com/problems/ways-to-split-array-into-three-subarrays/)

我们称一个分割整数数组的方案是 好的 ，当它满足：

数组被分成三个 非空 连续子数组，从左至右分别命名为 left ， mid ， right 。
left 中元素和小于等于 mid 中元素和，mid 中元素和小于等于 right 中元素和。
给你一个 非负 整数数组 nums ，请你返回 好的 分割 nums 方案数目。由于答案可能会很大，请你将结果对 10^9 + 7 取余后返回。

示例 1：
```
输入：nums = [1,1,1]
输出：1
解释：唯一一种好的分割方案是将 nums 分成 [1] [1] [1] 。
```

示例 2：
```
输入：nums = [1,2,2,2,5,0]
输出：3
解释：nums 总共有 3 种好的分割方案：
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]
```

示例3：
```
输入：nums = [3,2,1]
输出：0
解释：没有好的分割方案。
```

提示：
- 1 <= nums.length, k <= 10^5
- -10^4 <= nums[i] <= 10^4

### 思路

前缀和 + 二分

- 双指针保证中间区间的和不小于左边区间
- 二分查找能够使得中间区间的和不大于右边区间的最右位置。
### 代码
两层循环，超时
```c++
class Solution {
public:
    int waysToSplit(vector<int>& nums) {
        long long res = 0;
        const int MOD = 1e9 + 7;
        int n = nums.size();
        vector<int> S(n+1,0);
        // 前缀和
        for (int i = 1; i <= n; i++) {
            S[i] = S[i-1] + nums[i-1];
        }
        int l = 0, r = 0;
        for (int l = 1; l < n - 1; l++) {
            for (int r = l + 1; r <= n; r++) {
                int right = S[n] - S[r];
                int mid = S[r] - S[l];
                int left = S[l] - S[0];
                if (left <= mid && mid <= right) {
                    res++;
                }
            }
        }
        return res % MOD;
    }
};
```

二分优化, O(nlogn)
```c++
class Solution {
public:
    int waysToSplit(vector<int>& nums) {
        long long res = 0;
        const int MOD = 1e9 + 7;
        int n = nums.size();
        vector<int> S(n+1,0);
        for (int i = 1; i <= n; i++) {
            S[i] = S[i-1] + nums[i-1];
        }
        int l = 0, r = 0;
        int m = 2;
        for (int l = 1; l < n - 1; l++) {
            int left = S[l] - S[0];
            m = max(m, l + 1);
            // 寻找左边界
            while (m < n && S[m] - S[l] < left) m++;
            if (m == n) break;
            int low = m, high = n;
            // 寻找右边界
            while (low < high) {
                r = (low + high) / 2;
                int right = S[n] - S[r];
                int mid = S[r] - S[l];
                if (mid > right) {
                    high = r;
                } else {
                    low = r + 1;
                }
            }
            res += high - 1 - m + 1;
        }
        return res % MOD;
    }
};
```

三指针, O(n)
```c++
typedef long long ll;
const ll MOD = 1e9 + 7;

class Solution {
public:
    int waysToSplit(vector<int>& nums) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; ++i)
            s[i] = s[i - 1] + nums[i - 1];
        ll ans = 0;
        int l = 2, r = 2;
        for (int m = 2; m <= n - 1; ++m) {
            l = max(l, m), r = max(r, m);
            while (r + 1 < n && s[n] - s[r + 1] >= s[r + 1] - s[m - 1])
                r++;
            while (l < n && s[l] - s[m - 1] < s[m - 1])
                l++;
            if (r < n && l <= r && s[l] - s[m - 1] >= s[m - 1] && s[n] - s[r] >= s[r] - s[m - 1])
                ans += r - l + 1;
        }
        return ans % MOD;
    }
};
```

### [1713. 得到子序列的最少操作次数](https://leetcode-cn.com/problems/minimum-operations-to-make-a-subsequence/)

给你一个数组 target ，包含若干 互不相同 的整数，以及另一个整数数组 arr ，arr 可能 包含重复元素。

每一次操作中，你可以在 arr 的任意位置插入任一整数。比方说，如果 arr = [1,4,1,2] ，那么你可以在中间添加 3 得到 [1,4,3,1,2] 。你可以在数组最开始或最后面添加整数。

请你返回 最少 操作次数，使得 target 成为 arr 的一个子序列。

一个数组的 子序列 指的是删除原数组的某些元素（可能一个元素都不删除），同时不改变其余元素的相对顺序得到的数组。比方说，[2,7,4] 是 [4,2,3,7,2,1,4] 的子序列（加粗元素），但 [2,4,2] 不是子序列。

示例 1：
```
输入：target = [5,1,3], arr = [9,4,2,3,4]
输出：2
解释：你可以添加 5 和 1 ，使得 arr 变为 [5,9,4,1,2,3,4] ，target 为 arr 的子序列。
```

示例 2：
```
输入：target = [6,4,8,1,3,2], arr = [4,7,6,2,3,8,6,1]
输出：3
```

提示：
- 1 <= target.length, arr.length <= 105
- 1 <= target[i], arr[i] <= 109
- target 不包含任何重复元素。

### 思路
最长递增子序列


### 代码
```c++
class Solution {
public:
    int minOperations(vector<int>& target, vector<int>& arr) {
        //按target的顺序来，就是按索引升序
        //给arr里面对应的target里的值标记索引，然后求最长升序子序列
        unordered_map<int, int> hash;
        for(int i = 0; i < target.size(); ++i){
            hash[target[i]] = i;
        }
        //给arr中target元素标号
        vector<int> copy;
        for(auto& i : arr){
            if(hash.find(i) != hash.end()){
                copy.emplace_back(hash[i]);
            }
        }
        //最长升序子序列, nlogn
        vector<int> minList;
        for(auto& i : copy){
            if(minList.empty() || i > minList.back()){
                minList.emplace_back(i);
            }else{
                *lower_bound(minList.begin(), minList.end(), i) = i;
            }
        }
        return target.size() - minList.size();
    }
};
```