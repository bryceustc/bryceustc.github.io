---
title: 【Leetcode】第256场周赛
date: 2021-08-29 20:47:36
tags: LeetCode
categories: LeetCode
---

### [5854. 学生分数的最小差值](https://leetcode-cn.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)

给你一个 **下标从 0 开始** 的整数数组 nums ，其中 nums[i] 表示第 i 名学生的分数。另给你一个整数 k 。

从数组中选出任意 k 名学生的分数，使这 k 个分数间 **最高分** 和 **最低分** 的 **差值** 达到 **最小化** 。

返回可能的 **最小差值** 。

<!--more-->
示例1：
```
输入：nums = [90], k = 1
输出：0
解释：选出 1 名学生的分数，仅有 1 种方法：
- [90] 最高分和最低分之间的差值是 90 - 90 = 0
可能的最小差值是 0
```

示例2:
```
输入：nums = [9,4,1,7], k = 2
输出：2
解释：选出 2 名学生的分数，有 6 种方法：
- [9,4,1,7] 最高分和最低分之间的差值是 9 - 4 = 5
- [9,4,1,7] 最高分和最低分之间的差值是 9 - 1 = 8
- [9,4,1,7] 最高分和最低分之间的差值是 9 - 7 = 2
- [9,4,1,7] 最高分和最低分之间的差值是 4 - 1 = 3
- [9,4,1,7] 最高分和最低分之间的差值是 7 - 4 = 3
- [9,4,1,7] 最高分和最低分之间的差值是 7 - 1 = 6
可能的最小差值是 2
```

提示：
- 1 <= k <= nums.length <= 1000
- 0 <= nums[i] <= $10^5$

### 思路
一开始想复杂了，一开始采用dfs了，其实排序后滑动窗口即可。

### 代码
```c++
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int res = 100005;
        if (n == 1) return 0;
        for (int i = 0; i <= n - k; i++) {
            int tmp = nums[i+k-1] - nums[i];
            res = min(res, tmp);
        }
        return res;
    }
};
```

### [5855. 找出数组中的第 K 大整数](https://leetcode-cn.com/problems/find-the-kth-largest-integer-in-the-array/)

给你一个字符串数组 nums 和一个整数 k 。nums 中的每个字符串都表示一个不含前导零的整数。

返回 nums 中表示第 k 大整数的字符串。

注意：重复的数字在统计时会视为不同元素考虑。例如，如果 nums 是 ["1","2","2"]，那么 "2" 是最大的整数，"2" 是第二大的整数，"1" 是第三大的整数。

示例 1：
```
输入：nums = ["3","6","7","10"], k = 4
输出："3"
解释：
nums 中的数字按非递减顺序排列为 ["3","6","7","10"]
其中第 4 大整数是 "3"
```

示例 2：
```
输入：nums = ["2","21","12","1"], k = 3
输出："2"
解释：
nums 中的数字按非递减顺序排列为 ["1","2","12","21"]
其中第 3 大整数是 "2"
```

示例 3：
```
输入：nums = ["0","0"], k = 2
输出："0"
解释：
nums 中的数字按非递减顺序排列为 ["0","0"]
其中第 2 大整数是 "0"
```

提示：
- 1 <= k <= nums.length <= 104
- 1 <= nums[i].length <= 100
- nums[i] 仅由数字组成
- nums[i] 不含任何前导零

### 思路
1. 我们将字符串数组按照字符串转换后的整数的大小进行排序。
2. 排序比较方法如下：
    - 如果字符串与的长度不相等，则长度较大的转换为整数后较大。
    - 如果字符串与的长度相等，则比较两个字符串的字典序，字典序较大的转换为整数后较大。
3. 算法的时间复杂度为 ,其中  为字符串数组的长度， 为字符串的平均长度。

PS：这里比赛的时候使用堆排序，但是会存在大数越界的问题，字符串问题以后要注意。
### 代码
```c++
//堆排序的做法，未全部ac
class Solution {
public:
    string kthLargestNumber(vector<string>& nums, int k) {
        int n = nums.size();
        priority_queue<long long> q;
        for (int i = 0; i < n; i++) {
            long long a = stoll(nums[i]);
            q.push(a);
            if (q.size() > n - (k-1)) {
                q.pop();
            }
        }
        string res = to_string(q.top());
        return res;
    }
};
// 字符串
class Solution {
public:
    string kthLargestNumber(vector<string>& nums, int k) {
        int n = nums.size();
        sort(nums.begin(), nums.end(), cmp);
        string res = nums[n-k];
        return res;
    }
    static bool cmp(string &a, string &b) {
        if (a.size() < b.size() ) {
            return true;
        } else if (a.size() > b.size()) {
            return false;
        } else if (a.size() == b.size()) {
            for (int i = 0; i < a.size(); i++) {
                if (a[i]< b[i]) {
                    return true;
                } else if (a[i] > b[i]) {
                    return false;
                } else if (a[i] == b[i]) {
                    continue;
                }
            }
        }
        return false;
    }
};

class Solution {
public:
    string kthLargestNumber(vector<string>& nums, int k) {
        sort(nums.rbegin(), nums.rend(), [](string& a, string& b) {
            return (a.size() != b.size()) ? a.size() < b.size() : a < b;
        });

        return nums[k - 1];
    }
};
```

### [5856. 完成任务的最少工作时间段](https://leetcode-cn.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

你被安排了 n 个任务。任务需要花费的时间用长度为 n 的整数数组 tasks 表示，第 i 个任务需要花费 tasks[i] 小时完成。一个**工作时间段**中，你可以**至多**连续工作 sessionTime 个小时，然后休息一会儿。

你需要按照如下条件完成给定任务：

- 如果你在某一个时间段开始一个任务，你需要在 同一个 时间段完成它。
- 完成一个任务后，你可以**立马**开始一个新的任务。
- 你可以按**任意顺序** **完成任务。

给你 tasks 和 sessionTime ，请你按照上述要求，返回完成所有任务所需要的**最少**数目的**工作时间段**。

测试数据保证 sessionTime**大于等于**tasks[i] 中的**最大值**。

示例 1：
```
输入：tasks = [1,2,3], sessionTime = 3
输出：2
解释：你可以在两个工作时间段内完成所有任务。
- 第一个工作时间段：完成第一和第二个任务，花费 1 + 2 = 3 小时。
- 第二个工作时间段：完成第三个任务，花费 3 小时。
```

示例 2：
```
输入：tasks = [3,1,3,1,1], sessionTime = 8
输出：2
解释：你可以在两个工作时间段内完成所有任务。
- 第一个工作时间段：完成除了最后一个任务以外的所有任务，花费 3 + 1 + 3 + 1 = 8 小时。
- 第二个工作时间段，完成最后一个任务，花费 1 小时。
```
示例 3：
```
输入：tasks = [1,2,3,4,5], sessionTime = 15
输出：1
解释：你可以在一个工作时间段以内完成所有任务。
```


提示：
- n == tasks.length
- 1 <= n <= 14
- 1 <= tasks[i] <= 10
- max(tasks[i]) <= sessionTime <= 15

### 思路

状压 DP 就是枚举所有情况的 「 暴力 」。

本题的数据范围 1 <= n <= 14 在隐约地告诉我们，需要暴力地将所有情况算出来，取最优解。

**Tips：以普遍理性而论，状压的数据范围都会小于 20.**

状态压缩基础知识：

1. 数组长度为n, 所有的组合有N = 2^n, [0, N-1], 共有N种状态，
n = 3, 状态有000, 001, 010, 011, 100, 101, 110, 110，
对于状态i = 101, 状态i的子集j的概念, j = 000, 001, 100，用公式表示 i | j = i
2. 对于本题，dp[i]表示完成任务i, 所需的最小session数；
预处理所有只需要一个session的情况，即dp[i] = 1的情况；
递推公式，dp[i] = dp[i-j] + dp[j], 等于子集的session相加；

### 代码
```c++
class Solution {
    public int minSessions(int[] tasks, int sessionTime) {
        int n = tasks.length, m = 1 << n;
        final int INF = 20;
        int[] dp = new int[m];
        Arrays.fill(dp, INF);

        // 预处理每个状态，合法状态预设为 1
        for (int i = 1; i < m; i++) {
            int state = i, idx = 0;
            int spend = 0;
            while (state > 0) {
                int bit = state & 1;
                if (bit == 1) {
                    spend += tasks[idx];
                }
                state >>= 1;
                idx++;
            }
            if (spend <= sessionTime) {
                dp[i] = 1;
            }
        }

        // 对每个状态枚举子集，跳过已经有最优解的状态
        for (int i = 1; i < m; i++) {
            if (dp[i] == 1) {
                continue;
            }
            for (int j = i; j > 0; j = (j - 1) & i) {
                // i 状态的最优解可能由当前子集 j 与子集 j 的补集得来
                dp[i] = Math.min(dp[i], dp[j] + dp[i ^ j]);
            }
        }

        return dp[m - 1];
    }
}
```

### [5857. 不同的好子序列数目](https://leetcode-cn.com/problems/number-of-unique-good-subsequences/)

给你一个二进制字符串 binary 。 binary 的一个 子序列 如果是 非空 的且没有 前导 0 （除非数字是 "0" 本身），那么它就是一个 好 的子序列。

请你找到 binary 不同好子序列 的数目。

比方说，如果 binary = "001" ，那么所有 好 子序列为 ["0", "0", "1"] ，所以 不同 的好子序列为 "0" 和 "1" 。 注意，子序列 "00" ，"01" 和 "001" 不是好的，因为它们有前导 0 。
请你返回 binary 中 不同好子序列 的数目。由于答案可能很大，请将它对 109 + 7 取余 后返回。

一个 子序列 指的是从原数组中删除若干个（可以一个也不删除）元素后，不改变剩余元素顺序得到的序列。

示例 1：
```
输入：binary = "001"
输出：2
解释：好的二进制子序列为 ["0", "0", "1"] 。
不同的好子序列为 "0" 和 "1" 。
```

示例 2：
```
输入：binary = "11"
输出：2
解释：好的二进制子序列为 ["1", "1", "11"] 。
不同的好子序列为 "1" 和 "11" 。
```

示例3：
```
输入：binary = "101"
输出：5
解释：好的二进制子序列为 ["1", "0", "1", "10", "11", "101"] 。
不同的好子序列为 "0" ，"1" ，"10" ，"11" 和 "101" 。

```

提示：
- 1 <= binary.length <= $10^5$
- binary 只含有 '0' 和 '1' 。

### 思路
动态规划

**1. 定义**
定义

因为我们关心子序列的 开头 是什么，所以我们这样定义：

- dp[i][0]= 字符串 binary 的从 i 开始的子串中，以 0 开头的子序列的个数。

- dp[i][1]= 字符串 binary 的从 i 开始的子串中，以 1 开头的子序列的个数。

**2. 状态转移**

我们从后向前遍历字符串 binary 。

- 当 binary[i] == '0' 时，dp[i][0] 的求解可以分成 3 个部分：

    - 第一部分：这个 '0' 可以添加到所有的子序列的前面，此部分共有 dp[i+1][0]+dp[i+1][1] 个不同的子序列；

    - 第二部分：原有的 dp[i+1][0] 个以 0 开始的子序列，但是这部分不能增加到 dp[i][0] 中，因为这些序列肯定包含在第一部分和第三部分中，不能重复添加；

    - 第三部分：单独的 1 个 0。

因此，dp[i][0] = 第一部分 + 第三部分 =dp[i+1][0]+dp[i+1][1]+1。

然后，由于 binary[i] == '0'，故 dp[i][1] 不会发生变化，dp[i][1]=dp[i+1][1]。

当 binary[i] == '1' 时，同理可推：
- dp[i][1]=dp[i+1][0]+dp[i+1][1]+1
- dp[i][0]=dp[i+1][0]

**3. 最终答案**

最终答案就是，全部以 1 开头的子序列的个数（也就是 dp[0][1]）+ 字符串 “0” （如果有）。

代码实现采用了滚动数组的方式，空间复杂度为 O(1)，时间复杂度为 O(n)。

### 代码
```c++
class Solution {
public:
    int numberOfUniqueGoodSubsequences(string binary) {
        int n = binary.size();
        int dp0 = 0, dp1 = 0, mod = 1e9 + 7;
        // 是否包含0字符
        int has0 = 0;
        for (int i = n-1; i >=0; i--) {
            if (binary[i] == '0') {
                has0 = 1;
                dp0 = (dp0 + dp1 + 1) % mod;
            } else {
                dp1 = (dp0 + dp1 + 1) % mod;
            }
        }
        return dp1 + has0;
    }
};


class Solution {
public:
    int numberOfUniqueGoodSubsequences(string binary) {
        int n = binary.size();
        long long mod = 1e9 + 7;
        bool has_zero = false;
        vector<long long> dp(2);

        for(int i = n - 1; i >= 0; --i){
            int x = binary[i] - '0';
            if(x == 0){ 
                has_zero = true;
                dp[0] = (1 + dp[0] + dp[1])%mod;
                dp[1] = dp[1];
            }else{
                dp[0] = dp[0];
                dp[1] = (1 + dp[0] + dp[1])%mod;
            }
        }
        if(has_zero){
            dp[1] = (dp[1] + 1)%mod;
        }

        return dp[1];
    }
};
```