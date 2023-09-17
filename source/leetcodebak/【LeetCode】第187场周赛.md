---
title: 【LeetCode】第187场周赛
date: 2020-05-03 14:18:24
tags: LeetCode
categories: LeetCode
---
## 第187场周赛

本周题目AC 2.9/4，第三题滑动窗口超时，本周题目不算很难。


### [5400. 旅行终点站](https://leetcode-cn.com/problems/destination-city/)

给你一份旅游线路图，该线路图中的旅行线路用数组 paths 表示，其中 paths[i] = [cityAi, cityBi] 表示该线路将会从 cityAi 直接前往 cityBi 。请你找出这次旅行的终点站，即没有任何可以通往其他城市的线路的城市。
<!--more-->
题目数据保证线路图会形成一条不存在循环的线路，因此只会有一个旅行终点站。

示例1：
```
输入：paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
输出："Sao Paulo" 
解释：从 "London" 出发，最后抵达终点站 "Sao Paulo" 。本次旅行的路线是 "London" -> "New York" -> "Lima" -> "Sao Paulo" 。
```

示例2:
```
输入：paths = [["B","C"],["D","B"],["C","A"]]
输出："A"
解释：所有可能的线路是：
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
显然，旅行终点站是 "A" 。
```

示例3：
```
输入：paths = [["A","Z"]]
输出："Z"
```

### 思路

利用哈希表计算出度为0的节点

### 代码
```c++
class Solution {
public:
    string destCity(vector<vector<string>>& paths) {
        int n = paths.size();
        string res;
        unordered_map<string,vector<string>> m;
        for (int i=0;i<n;i++)
        {
            m[paths[i][0]].push_back(paths[i][1]);
        }
        for (int i=0;i<n;i++)
        {
            string a = paths[i][1];
            if (m.count(a)==0)
            {
                res = a;
                break;
            }
        }
        return res;
    }
};
```

### [5401. 是否所有 1 都至少相隔 k 个元素](https://leetcode-cn.com/problems/check-if-all-1s-are-at-least-length-k-places-away/)

给你一个由若干 0 和 1 组成的数组 nums 以及整数 k。如果所有 1 都至少相隔 k 个元素，则返回 True ；否则，返回 False 。

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/03/sample_1_1791.png)

```
输入：nums = [1,0,0,0,1,0,0,1], k = 2
输出：true
解释：每个 1 都至少相隔 2 个元素。
```

示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/03/sample_2_1791.png)
```
输入：nums = [1,0,0,1,0,1], k = 2
输出：false
解释：第二个 1 和第三个 1 之间只隔了 1 个元素。
```
示例 3：
```
输入：nums = [1,1,1,1,1], k = 0
输出：true
```

示例4：
```
输入：nums = [0,1,0,1], k = 1
输出：true
```

提示：
- 1 <= nums.length <= 10^5
- 0 <= k <= nums.length
- nums[i] 的值为 0 或 1

### 思路
顺序遍历一次记录上一次1出现的位置两者做差与k进行比较
### 代码
```
class Solution {
public:
    bool kLengthApart(vector<int>& nums, int k) {
        int n = nums.size();
        int dis = 0;
        int start = -1;
        for (int i=0;i<n;i++)
        {
            if (nums[i]==1)
            {
                if (start==-1)
                {
                    start=i;
                }
                else
                {
                    dis = i-start-1;
                    if (dis<k) return false;
                    start = i;
                }
            }
        }
        return true;
    }
};
```

### [5402. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

给你一个整数数组 nums ，和一个表示限制的整数 limit，请你返回最长连续子数组的长度，该子数组中的任意两个元素之间的绝对差必须小于或者等于 limit 。

如果不存在满足条件的子数组，则返回 0 。

示例 1：

```
输入：nums = [8,2,4,7], limit = 4
输出：2 
解释：所有子数组如下：
[8] 最大绝对差 |8-8| = 0 <= 4.
[8,2] 最大绝对差 |8-2| = 6 > 4. 
[8,2,4] 最大绝对差 |8-2| = 6 > 4.
[8,2,4,7] 最大绝对差 |8-2| = 6 > 4.
[2] 最大绝对差 |2-2| = 0 <= 4.
[2,4] 最大绝对差 |2-4| = 2 <= 4.
[2,4,7] 最大绝对差 |2-7| = 5 > 4.
[4] 最大绝对差 |4-4| = 0 <= 4.
[4,7] 最大绝对差 |4-7| = 3 <= 4.
[7] 最大绝对差 |7-7| = 0 <= 4. 
因此，满足题意的最长子数组的长度为 2 。
```

示例 2：

```
输入：nums = [10,1,2,4,7,2], limit = 5
输出：4 
解释：满足题意的最长子数组是 [2,4,7,2]，其最大绝对差 |2-7| = 5 <= 5 。
```
示例 3：
```
输入：nums = [4,2,2,2,4,4,2,2], limit = 0
输出：3
```

提示：
- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^9
- 0 <= limit <= 10^9

### 思路
连续子数组、子串的问题一般都是考虑用滑动窗口来做，参考整理的[滑动窗口算法总结](https://bryceustc.github.io/2020/04/22/%E5%8F%8C%E6%8C%87%E9%92%88%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/)。

- 使用滑动窗口保持符合条件的子数组，记录最长长度
- 怎样确定子数组是否符合条件，需要知道两个关键数据
    - 子数组中的最大值
    - 子数组中的最小值
- 需要对滑入窗口的数据记录，滑出的数据删除，并且使这些记录方便的算出最大值和最小值
    - 使用 map / multiset 可以在滑入滑出的时候方便的增减对应数据
    - 同时 map / multiset 本身是有序的，可以方便的找出最大值最小值

### 代码

multiset
```c++
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        int res = 0;
        int left = 0, right = 0;
        int n = nums.size();
        int diff = 0, max_num=0, min_num=0;
        multiset<int> s;
        while(right<n)
        {
            s.insert(nums[right++]);
            max_num = *s.rbegin();
            min_num = *s.begin();
            diff = max_num -min_num;
            if (diff<=limit)
            {
                res = max(res, right-left);
                continue;
            }
            while(diff>limit)
            {
                s.erase(s.find(nums[left])); // s.erase(nums[left])会把所有的2删除，加上find只会删除最近的一个
                left++;
                max_num = *s.rbegin();
                min_num = *s.begin();
                diff = max_num -min_num;
            }
        }
        return res;
    }
};
```

map
```c++
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        int res =0;
        int left=0, right=0, n = nums.size();
        map<int,int> m;
        while(right<n)
        {
            m[nums[right]]++;
            right++;
            int max_num = m.rbegin()->first;
            int min_num = m.begin()->first;
            int diff = max_num-min_num;
            if (diff<=limit)
            {
                res = max(res,right-left);
                continue;
            }
            while(diff>limit)
            {
                m[nums[left]]--;
                if (m[nums[left]]==0)
                {
                    m.erase(nums[left]);
                }
                left++;
                diff = m.rbegin()->first - m.begin()->first;
            }
        }
        return res;
    }
};
```

### [5403. 有序矩阵中的第 k 个最小数组和](https://leetcode-cn.com/problems/find-the-kth-smallest-sum-of-a-matrix-with-sorted-rows/)

给你一个 m * n 的矩阵 mat，以及一个整数 k ，矩阵中的每一行都以非递减的顺序排列。

你可以从每一行中选出 1 个元素形成一个数组。返回所有可能数组中的第 k 个 最小数组和。

示例 1：

```
输入：mat = [[1,3,11],[2,4,6]], k = 5
输出：7
解释：从每一行中选出一个元素，前 k 个和最小的数组分别是：
[1,2], [1,4], [3,2], [3,4], [1,6]。其中第 5 个的和是 7 。  
```

示例 2：

```
输入：mat = [[1,3,11],[2,4,6]], k = 9
输出：17
```
示例 3：
```
输入：mat = [[1,10,10],[1,4,5],[2,3,6]], k = 7
输出：9
解释：从每一行中选出一个元素，前 k 个和最小的数组分别是：
[1,1,2], [1,1,3], [1,4,2], [1,4,3], [1,1,6], [1,5,2], [1,5,3]。其中第 7 个的和是 9 。 
```

示例4：
```
输入：mat = [[1,1,10],[2,2,9]], k = 7
输出：12
```

提示：
- m == mat.length
- n == mat.length[i]
- 1 <= m, n <= 40
- 1 <= k <= min(200, n ^ m)
- 1 <= mat[i][j] <= 5000
- mat[i] 是一个非递减数组

### 思路

此题比赛的时候没有思路，因为k比较小，可以使用暴力法来求解，参考[这里](https://leetcode-cn.com/problems/find-the-kth-smallest-sum-of-a-matrix-with-sorted-rows/solution/find-the-kth-smallest-sum-of-a-matrix-by-ikaruga/)

- 使用 vector<int> res 记录各个行选一个数相加的和
    - 记录的方法是，先保存第一行各数
    - 然后把第二行的各数拿出来，组合相加
    - 对其排列，超过 k 个就不需要保留了
    - 相加之后记录回 res ，下次拿出第三行各数，与其组合相加，以此类推
- 返回第 k 个最小数组和

**二分**

就是先确定左右边界，即最小和与最大和，然后二分得到mid，每次判断和小于mid的数组有多少个，如果大于等于k那么更新r，否则更新l。

### 代码

利用multiset，暴力法求解
```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& mat, int k) {
        int m = mat.size();
        vector<int> res(mat[0]);
        for (int i=1;i<m;i++)
        {
            multiset<int> s;
            for (int &x : res)
            {
                for (int &y : mat[i])
                {
                    s.insert(x+y);
                }
            }
            res.assign(s.begin(),s.end());
            res.resize(min(k, (int)res.size()));
        }
        return res[k-1];
    }
};
```

二分

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& mat, int k) {
        int left = 0, right = 0;
        int m = mat.size();
        for (int i=0;i<m;i++)
        {
            left+=mat[i].front();
            right+=mat[i].back();
        }
        int base = left;
        while(left<right)
        {
            int mid = left + (right-left)/2;
            int cnt = 1;
            dfs(mat, mid, base, 0, cnt, k);
            if (cnt>=k)
            {
                right = mid;
            }
            else
            {
                left = mid+1;
            }
        }
        return left;
    }
    void dfs(vector<vector<int>> &mat, int mid, int sum, int idx, int &cnt, int k)
    {
        if (idx == mat.size()) return;
        if (cnt>k || sum > mid) return;
        dfs(mat, mid, sum, idx+1, cnt, k);
        for (int i=1;i<mat[idx].size();i++)
        {
            int temp = sum + mat[idx][i] - mat[idx][0];
            if (temp<=mid)
            {
                cnt++;
                dfs(mat,mid, temp, idx+1,cnt, k);
            }
            else
            {
                break;
            }
        }
    }
};
```