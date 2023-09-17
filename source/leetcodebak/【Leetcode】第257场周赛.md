---
title: 【Leetcode】第257场周赛
date: 2021-09-05 13:28:12
tags: LeetCode
categories: LeetCode
---

### [5863. 统计特殊四元组](https://leetcode-cn.com/problems/count-special-quadruplets/)

给你一个 下标从 0 开始 的整数数组 nums ，返回满足下述条件的 不同 四元组 (a, b, c, d) 的 数目 ：
- nums[a] + nums[b] + nums[c] == nums[d] ，且
- a < b < c < d

<!--more-->
示例1：
```
输入：nums = [1,2,3,6]
输出：1
解释：满足要求的唯一一个四元组是 (0, 1, 2, 3) 因为 1 + 2 + 3 == 6 。
```

示例2:
```
输入：nums = [3,3,6,4,5]
输出：0
解释：[3,3,6,4,5] 中不存在满足要求的四元组。
```

示例3:
```
输入：nums = [1,1,1,3,5]
输出：4
解释：满足要求的 4 个四元组如下：
- (0, 1, 2, 3): 1 + 1 + 1 == 3
- (0, 1, 3, 4): 1 + 1 + 3 == 5
- (0, 2, 3, 4): 1 + 1 + 3 == 5
- (1, 2, 3, 4): 1 + 1 + 3 == 5
```

提示：
- 4 <= nums.length <= 50
- 1 <= nums[i] <= 100

### 思路
一开始想复杂了，先sort了，直接暴力遍历模拟即可，一般第一题直接暴力模拟就行。

### 代码
```c++
class Solution {
public:
    int countQuadruplets(vector<int>& nums) {
        int count = 0;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    for (int l = k + 1; l < n; l++) {
                        if (nums[i] + nums[j] + nums[k] == nums[l]) {
                            count++;
                        }
                    }
                }
            }
        }
        return count;
    }
};
```

### [5864. 游戏中弱角色的数量](https://leetcode-cn.com/problems/the-number-of-weak-characters-in-the-game/)

你正在参加一个多角色游戏，每个角色都有两个主要属性：攻击 和 防御 。给你一个二维整数数组 properties ，其中 $properties[i] = [attack_i, defense_i]$ 表示游戏中第 i 个角色的属性。

如果存在一个其他角色的攻击和防御等级 都严格高于 该角色的攻击和防御等级，则认为该角色为 弱角色 。更正式地，如果认为角色 i 弱于 存在的另一个角色 j ，那么 $attack_j > attack_i$ 且 $defense_j > defense_i$。

返回 弱角色 的数量。

示例 1：
```
输入：properties = [[5,5],[6,3],[3,6]]
输出：0
解释：不存在攻击和防御都严格高于其他角色的角色。
```

示例 2：
```
输入：properties = [[2,2],[3,3]]
输出：1
解释：第一个角色是弱角色，因为第二个角色的攻击和防御严格大于该角色。
```

示例 3：
```
输入：properties = [[1,5],[10,4],[4,3]]
输出：1
解释：第三个角色是弱角色，因为第二个角色的攻击和防御严格大于该角色。
```

提示：
- 2 <= properties.length <= 10^5
- properties[i].length == 2
- 1 <= $attack_i, defense_i$ <= 10^5

### 思路
1. 将角色按照攻击**从大到小**排序，攻击相同的按照防御**从小到大**排序。。
2. 遍历数组，维护遍历过的角色的防御的最大值 maxDef。
3. 对于当前角色 p，如果 p 的防御小于 maxDef，那么说明前面有防御比 p 高的角色（记作 q）；同时，根据上面的排序规则，如果 q 的攻击和 p 相同，那么 q 的防御不会超过 p，矛盾，因此 q 的攻击必然大于 p，这样 p 就是一个弱角色。
### 代码
```c++
class Solution {
public:
    int numberOfWeakCharacters(vector<vector<int>>& properties) {
        int n = properties.size();
        int count = 0;
        sort(properties.begin(), properties.end(), cmp);
        int maxDef = 0;
        for (int i = 0; i < n; i++) {
            if  (properties[i][1] < maxDef) {
                count++;
            } else {
                maxDef = properties[i][1];
            }
        }
        return count;
    }
    static bool cmp(vector<int> &A, vector<int> &B) {
        if (A[0] > B[0] || (A[0] == B[0] && A[1] < B[1])) {
            return true;
        }
        return false;
    }
};
```

### [5865. 访问完所有房间的第一天](https://leetcode-cn.com/problems/first-day-where-you-have-been-in-all-the-rooms/)

你需要访问 n 个房间，房间从 0 到 n - 1 编号。同时，每一天都有一个日期编号，从 0 开始，依天数递增。你每天都会访问一个房间。

最开始的第 0 天，你访问 0 号房间。给你一个长度为 n 且 下标从 0 开始 的数组 nextVisit 。在接下来的几天中，你访问房间的 次序 将根据下面的 规则 决定：

- 假设某一天，你访问 i 号房间。
- 如果算上本次访问，访问 i 号房间的次数为 奇数 ，那么 第二天 需要访问 nextVisit[i] 所指定的房间，其中 0 <= nextVisit[i] <= i 。
- 如果算上本次访问，访问 i 号房间的次数为 偶数 ，那么 第二天 需要访问 (i + 1) mod n 号房间。

请返回你访问完所有房间的第一天的日期编号。题目数据保证总是存在这样的一天。由于答案可能很大，返回对 10^9 + 7 取余后的结果。

示例 1：
```
输入：nextVisit = [0,0]
输出：2
解释：
- 第 0 天，你访问房间 0 。访问 0 号房间的总次数为 1 ，次数为奇数。
  下一天你需要访问房间的编号是 nextVisit[0] = 0
- 第 1 天，你访问房间 0 。访问 0 号房间的总次数为 2 ，次数为偶数。
  下一天你需要访问房间的编号是 (0 + 1) mod 2 = 1
- 第 2 天，你访问房间 1 。这是你第一次完成访问所有房间的那天。
```

示例 2：
```
输入：nextVisit = [0,0,2]
输出：6
解释：
你每天访问房间的次序是 [0,0,1,0,0,1,2,...] 。
第 6 天是你访问完所有房间的第一天。
```
示例 3：
```
输入：nextVisit = [0,1,2,0]
输出：6
解释：
你每天访问房间的次序是 [0,0,1,1,2,2,3,...] 。
第 6 天是你访问完所有房间的第一天。
```


提示：
- n == nextVisit.length
- 2 <= n <= 10^5
- 0 <= nextVisit[i] <= i

### 思路

根据题目给出的规则，奇数只会停在当前或者往前走 + 偶数才会往后走。

推出 ->
- 最后一房间第一次访问时即全部房间被访问过
- 第一次访问到某间房时，其余房一定访问过偶数次

dp定义：
dp[i] -> 第i间房第一次访问时的天数

dp转移：
从i-1到i，根据前置规则可知，需要i-1为偶数次访问，即dp[i] = 第二次访问i-1房天数 + 1

而第二次访问i-1房天数如何计算？
在奇数访问i-1时，要观察下一跳如何走，才能推导出第二次走回i-1的公式：

- 当next[i-1] == i-1, 下一跳即可第二次访问i-1，-> dp[i-1] + 1
- 当next[i-1] < i-1, 下一跳访问回到next[i-1]，此时需要额外再从next[i-1]走到i-1，这个时刻下next[i-1]和i-1都是奇数次访问状态，即第一次访问天数计算差值即可 -> dp[i-1] + 1 + (dp[i-1] - dp[next[i-1]])

故，dp转移方程
- dp[i-1] + 2, if next[i-1] == i-1
- 2 * dp[i-1] - dp[next[i-1]] + 2, if next[i-1] < i-1

整理可得：
- 2 * dp[i-1] - dp[next[i-1]] + 2

### 代码
```c++
class Solution {
public:
    int firstDayBeenInAllRooms(vector<int>& nextVisit) {
        //dp[i] 代表可以房间0到房间i的天数
        //到i房间之前必然先到i-1房间，即为dp[i-1]
        //现在考虑从i-1房间到i房间所可能经历的过程
        //1：若next[i-1] = i-1; 我们下一天还在原地，第二天可以到i
        //故所花时间2天 
        //dp[i] = 2 + dp[i-1]
        //2: 若不等，我们需要跳到 next[i-1] 房间，从next[i-1]房间最终再次
        //跳到i-1房间，使得房间访问次数奇偶变化 才能向右移到i房间
        //从i-1跳到next[i-1]需要1天，从next[i-1]跳回i-1需要dp[i-1]-dp[next[i-1]]天，最后从i-1跳到i需要最后1天.
        //故从i-1完整跳到i需要 2 + dp[i-1] - dp[next[i-1]];
        //dp[i] = dp[i-1]+2+dp[i-1]-dp[next[i-1]]
        //        = 2+2*dp[i-1] - dp[next[i-1]];
        //综合可以总结为dp[i] = 2 + 2*dp[i-1] - dp[next[i-1]];
        //因为数很大，所以取模
        int n = nextVisit.size();
        const int mod = 1e9 + 7;
        vector<int> dp(n);
        for (int i = 1; i < n; i++) {
            dp[i] = (2L + 2 * dp[i-1] - dp[nextVisit[i-1]] + mod) % mod;
        } 
        return dp[n-1];
    }
};
```

### [1998. 数组的最大公因数排序](https://leetcode-cn.com/problems/gcd-sort-of-an-array/)

给你一个整数数组 nums ，你可以在 nums 上执行下述操作 任意次 ：

- 如果 gcd(nums[i], nums[j]) > 1 ，交换 nums[i] 和 nums[j] 的位置。其中 gcd(nums[i], nums[j]) 是 nums[i] 和 nums[j] 的最大公因数。

如果能使用上述交换方式将 nums 按 非递减顺序 排列，返回 true ；否则，返回 false 。

示例 1：
```
输入：nums = [7,21,3]
输出：true
解释：可以执行下述操作完成对 [7,21,3] 的排序：
- 交换 7 和 21 因为 gcd(7,21) = 7 。nums = [21,7,3]
- 交换 21 和 3 因为 gcd(21,3) = 3 。nums = [3,7,21]
```

示例 2：
```
输入：nums = [5,2,6,2]
输出：false
解释：无法完成排序，因为 5 不能与其他元素交换。
```

示例3：
```
输入：nums = [10,5,9,3,15]
输出：true
解释：
可以执行下述操作完成对 [10,5,9,3,15] 的排序：
- 交换 10 和 15 因为 gcd(10,15) = 5 。nums = [15,5,9,3,10]
- 交换 15 和 3 因为 gcd(15,3) = 3 。nums = [3,5,9,15,10]
- 交换 10 和 15 因为 gcd(10,15) = 5 。nums = [3,5,9,10,15]
```

提示：
- 1 <= nums.length <= 3 * 10^4
- 2 <= nums[i] <= 10^5

### 思路
并查集+分解质因数

1.题意为任意两个数的公因数大于1，那么这两个数可以交换。由此可知如果a和b可交换，b和c可交换，那么a,b,c三者可以任意改变顺序，不难想到用并查集把所有公因数大于1的两个数合并。

2.如果用两层循环来判断来合并任意两个数，此时必然会超时。因此考虑将每个数和自己的所有质因子进行合并，如15和质因子3，5进行合并，21和质因子3，7合并，这样保证了21和15在同一个集合中。这样对于每个数仅仅需要分解质因子的时间复杂度，远远低于两层循环所需的时间复杂度。

3.合并之后，所有在一个并查集中的数可以任意交换。将原有的数组进行排序和新数组对比，如果原有数组和新数组的数字相同，则跳过，如果不同，则必须满足两个数在同一个并查集中，否则返回false，扫描一遍后如果没有返回false，则返回true.

### 代码
```c++
const int N = 3e5 + 10;

class Solution {
private:
    int p[N];
public:
    int find(int x) {
        if (x != p[x])  p[x] = find(p[x]);
        return p[x];
    }
    void merge(int a, int b) {
        int x = find(a), y = find(b);
        if (x == y)     return;
        p[x] = y;
    }
    bool gcdSort(vector<int>& nums) {
        vector<int> nums1 = nums;
        for (int i = 1; i < N; i++) p[i] = i;
        // 分解质因数
        for (auto c:nums) {
            int k = c;
            for (int i = 2; i <= c / i; i++) {
                bool flag = false;
                while (c % i == 0)
                    c /= i, flag = true;
                if (flag)
                    merge(k, i);
            }
            // 合并质因子
            if (c > 1)
               merge(k, c);
        }
        sort(nums1.begin(), nums1.end());
        // 对比原数组
        for (int i = 0; i < nums1.size(); i++) {
            if (nums1[i] == nums[i])    continue;
            if (find(nums1[i]) != find(nums[i]))    return false;
        }
        return true;
    }
};
```