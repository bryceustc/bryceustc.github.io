---
title: 【Leetcode】第214场周赛
date: 2020-11-08 11:58:12
tags: LeetCode
categories: LeetCode
mathjax: true
---
2020.11.8, 第214场周赛, ac 2/4 , 第三、四题dfs超时，排名932，三四题思路都有就是没想到优化点，都超时了，继续加油！
![](1.png)
### [5561. 获取生成数组中的最大值](https://leetcode-cn.com/problems/get-maximum-in-generated-array/)

给你一个整数 n 。按下述规则生成一个长度为 n + 1 的数组 nums ：

nums[0] = 0
nums[1] = 1
当 2 <= 2 * i <= n 时，nums[2 * i] = nums[i]
当 2 <= 2 * i + 1 <= n 时，nums[2 * i + 1] = nums[i] + nums[i + 1]
返回生成数组 nums 中的 最大值。

<!--more-->
示例1：
```
输入：n = 7
输出：3
解释：根据规则：
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
因此，nums = [0,1,1,2,1,3,2,3]，最大值 3
```

示例2:
```
输入：n = 2
输出：1
解释：根据规则，nums[0]、nums[1] 和 nums[2] 之中的最大值是 1
```

示例3：
```
输入：n = 3
输出：2
解释：根据规则，nums[0]、nums[1]、nums[2] 和 nums[3] 之中的最大值是 2
```

提示：
- 0 <= n <= 100

### 思路

暴力循环模拟即可。注意n==0的情形

### 代码
```c++
class Solution {
public:
    int getMaximumGenerated(int n) {
        int res = 0;
        vector<int> nums(n+1);
        nums[0] = 0;
        if (n == 0) return 0;
        nums[1] = 1;
        for (int i = 2; i <= n; i++) {
            if (i & 1) {
                int j = i / 2;
                nums[i] = nums[j] + nums[j+1];
            } else {
                nums[i] = nums[i/2];
            }
        }
        res = *max_element(nums.begin(), nums.end());
        return res;
    }
};
```

### [5562. 字符频次唯一的最小删除次数](https://leetcode-cn.com/problems/minimum-deletions-to-make-character-frequencies-unique/)

如果字符串 s 中 不存在 两个不同字符 频次 相同的情况，就称 s 是 优质字符串 。

给你一个字符串 s，返回使 s 成为 优质字符串 需要删除的 最小 字符数。

字符串中字符的 频次 是该字符在字符串中的出现次数。例如，在字符串 "aab" 中，'a' 的频次是 2，而 'b' 的频次是 1 。

示例 1：
```
输入：s = "aab"
输出：0
解释：s 已经是优质字符串。
```

示例 2：
```
输入：s = "aaabbbcc"
输出：2
解释：可以删除两个 'b' , 得到优质字符串 "aaabcc" 。
另一种方式是删除一个 'b' 和一个 'c' ，得到优质字符串 "aaabbc" 。
```
示例 3：
```
输入：s = "ceabaacb"
输出：2
解释：可以删除两个 'c' 得到优质字符串 "eabaab" 。
注意，只需要关注结果字符串中仍然存在的字符。（即，频次为 0 的字符会忽略不计。）
```

提示：
- 1 <= s.length <= 10^5
- s 仅含小写英文字母

### 思路
此题与[使数组唯一的最小增量](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Minimum-Increment-To-Make-Array-Unique/README.md)有点类似

首先得到每个字符出现的次数，并从大到小排序。然后从前到后，遍历这个频数数组。

遍历数组，若当前元素大于等于它的前一个元素,说明当前频数「过多」，需要去除一定数量的该字符。因为要尽可能少去除，因此需要保留 max{nums[i-1]−1,0}个字符。
### 代码

排序法

```c++
class Solution {
public:
    int minDeletions(string s) {
        int res = 0;
        int n = s.size();
        map<char, int> m;
        for (char &c : s) {
            m[c]++;
        }
        vector<int> nums;
        for (auto &p : m) {
            nums.push_back(p.second);
        }
        // 先降序排序
        sort(nums.begin(), nums.end(), greater<int>());
        // 遍历数组，若当前元素大于等于它的前一个元素，则将其变为前一个数-1
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] >= nums[i - 1]){
                int tmp = nums[i];
                nums[i] = max(0, nums[i-1] - 1);
                res += tmp - nums[i];
            }
        }
        return res;
    }
};
```
并查集法：

单链表式并查集

往前找到下一个没有出现的数字，对答案贡献为 当前数值 - 祖宗节点的差

```c++
const int N = 100010;
int p[N];

class Solution {
public:
    int minDeletions(string s) {
        int n = s.size();
        unordered_map<char,int> m;
        for(auto c : s) m[c] ++;
        for (int i = 1; i <= n; i++) p[i] = i;
        int res = 0;
        for (auto &c : m) {
            int num = c.second;
            int x = find(num);
            p[x] = max(0, x - 1);
            res += num - x;
        }
        return res;
    }
    int find (int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
};
```

### [5563. 销售价值减少的颜色球](https://leetcode-cn.com/problems/sell-diminishing-valued-colored-balls/)

你有一些球的库存 inventory ，里面包含着不同颜色的球。一个顾客想要 任意颜色 总数为 orders 的球。

这位顾客有一种特殊的方式衡量球的价值：每个球的价值是目前剩下的 同色球 的数目。比方说还剩下 6 个黄球，那么顾客买第一个黄球的时候该黄球的价值为 6 。这笔交易以后，只剩下 5 个黄球了，所以下一个黄球的价值为 5 （也就是球的价值随着顾客购买同色球是递减的）

给你整数数组 inventory ，其中 inventory[i] 表示第 i 种颜色球一开始的数目。同时给你整数 orders ，表示顾客总共想买的球数目。你可以按照 任意顺序 卖球。

请你返回卖了 orders 个球以后 最大 总价值之和。由于答案可能会很大，请你返回答案对 109 + 7 取余数 的结果。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/08/jj.gif)
```
输入：inventory = [2,5], orders = 4
输出：14
解释：卖 1 个第一种颜色的球（价值为 2 )，卖 3 个第二种颜色的球（价值为 5 + 4 + 3）。
最大总和为 2 + 5 + 4 + 3 = 14 。
```

示例 2：
```
输入：inventory = [3,5], orders = 6
输出：19
解释：卖 2 个第一种颜色的球（价值为 3 + 2），卖 4 个第二种颜色的球（价值为 5 + 4 + 3 + 2）。
最大总和为 3 + 2 + 5 + 4 + 3 + 2 = 19 。
```

示例 3:
```
输入：inventory = [2,8,4,10,6], orders = 20
输出：110
```

示例 4:
```
输入：inventory = [1000000000], orders = 1000000000
输出：21
解释：卖 1000000000 次第一种颜色的球，总价值为 500000000500000000 。 500000000500000000 对 109 + 7 取余为 21 。
```

提示：
- 1 <= inventory.length <= 105
- 1 <= inventory[i] <= 109
- 1 <= orders <= min(sum(inventory[i]), 109)

### 思路

首先，贪心的思路很容易想到：我们每次会找到当前剩余最多的那一类球（如果有多个类剩余的球数相同，那么任意选择一个类即可），然后将一个这类的球卖给顾客。我们连续这样操作 orders次，就可以卖出最大的价值。但是这样暴力会超时。

先用二分查找定位最低价格，再计算总额
### 代码

```c++
class Solution {
private:
    using LL = long long;
    
    static constexpr int mod = 1000000007;
    
    // 等差数列求和
    static constexpr LL rangeSum(int x, int y) {
        return static_cast<LL>(x + y) * (y - x + 1) / 2;
    }

public:
    int maxProfit(vector<int>& inventory, int orders) {
        int l = 0;
        int r = *max_element(inventory.begin(), inventory.end());
        int T = -1;
        while (l <= r) {
            int mid = (l + r) / 2;
            LL total = accumulate(inventory.begin(), inventory.end(), 0LL, [&](LL acc, int ai) {
                return acc + max(ai - mid, 0);
            });
            if (total <= orders) {
                T = mid;
                r = mid - 1;
            }
            else {
                l = mid + 1;
            }
        }
        int rest = orders - accumulate(inventory.begin(), inventory.end(), 0, [&](int acc, int ai) {
            return acc + max(ai - T, 0);
        }); // 最后是累加的方式
        LL ans = 0;
        for (int ai: inventory) {
            if (ai >= T) {
                if (rest > 0) {
                    ans += rangeSum(T, ai);
                    --rest;
                }
                else {
                    ans += rangeSum(T + 1, ai);
                }
            }
        }
        return ans % mod;
    }
};
```

### [5564. 通过指令创建有序数组](https://leetcode-cn.com/problems/create-sorted-array-through-instructions/)

给你一个整数数组 instructions ，你需要根据 instructions 中的元素创建一个有序数组。一开始你有一个空的数组 nums ，你需要 从左到右 遍历 instructions 中的元素，将它们依次插入 nums 数组中。每一次插入操作的 代价 是以下两者的 较小值 ：

nums 中 严格小于  instructions[i] 的数字数目。
nums 中 严格大于  instructions[i] 的数字数目。
比方说，如果要将 3 插入到 nums = [1,2,3,5] ，那么插入操作的 代价 为 min(2, 1) (元素 1 和  2 小于 3 ，元素 5 大于 3 ），插入后 nums 变成 [1,2,3,3,5] 。

请你返回将 instructions 中所有元素依次插入 nums 后的 总最小代价 。由于答案会很大，请将它对 109 + 7 取余 后返回。

示例 1：
```
输入：instructions = [1,5,6,2]
输出：1
解释：一开始 nums = [] 。
插入 1 ，代价为 min(0, 0) = 0 ，现在 nums = [1] 。
插入 5 ，代价为 min(1, 0) = 0 ，现在 nums = [1,5] 。
插入 6 ，代价为 min(2, 0) = 0 ，现在 nums = [1,5,6] 。
插入 2 ，代价为 min(1, 2) = 1 ，现在 nums = [1,2,5,6] 。
总代价为 0 + 0 + 0 + 1 = 1 。
```

示例 2：
```
输入：instructions = [1,2,3,6,5,4]
输出：3
解释：一开始 nums = [] 。
插入 1 ，代价为 min(0, 0) = 0 ，现在 nums = [1] 。
插入 2 ，代价为 min(1, 0) = 0 ，现在 nums = [1,2] 。
插入 3 ，代价为 min(2, 0) = 0 ，现在 nums = [1,2,3] 。
插入 6 ，代价为 min(3, 0) = 0 ，现在 nums = [1,2,3,6] 。
插入 5 ，代价为 min(3, 1) = 1 ，现在 nums = [1,2,3,5,6] 。
插入 4 ，代价为 min(3, 2) = 2 ，现在 nums = [1,2,3,4,5,6] 。
总代价为 0 + 0 + 0 + 0 + 1 + 2 = 3 。
```

示例3：
```
输入：instructions = [1,3,3,3,2,4,2,1,2]
输出：4
解释：一开始 nums = [] 。
插入 1 ，代价为 min(0, 0) = 0 ，现在 nums = [1] 。
插入 3 ，代价为 min(1, 0) = 0 ，现在 nums = [1,3] 。
插入 3 ，代价为 min(1, 0) = 0 ，现在 nums = [1,3,3] 。
插入 3 ，代价为 min(1, 0) = 0 ，现在 nums = [1,3,3,3] 。
插入 2 ，代价为 min(1, 3) = 1 ，现在 nums = [1,2,3,3,3] 。
插入 4 ，代价为 min(5, 0) = 0 ，现在 nums = [1,2,3,3,3,4] 。
​​​​​插入 2 ，代价为 min(1, 4) = 1 ，现在 nums = [1,2,2,3,3,3,4] 。
插入 1 ，代价为 min(0, 6) = 0 ，现在 nums = [1,1,2,2,3,3,3,4] 。
插入 2 ，代价为 min(2, 4) = 2 ，现在 nums = [1,1,2,2,2,3,3,3,4] 。
总代价为 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4 。
```

提示：
- 1 <= instructions.length <= 10^5
- 1 <= instructions[i] <= 10^5

### 思路
此题是个模板题。

这题的关键是找到数组中小于和大于某个数num的数。此题是要利用树状数组。注意这个知识点

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