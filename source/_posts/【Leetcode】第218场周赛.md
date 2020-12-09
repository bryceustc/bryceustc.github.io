---
title: 【Leetcode】第218场周赛
date: 2020-12-09 12:18:33
tags: LeetCode
categories: LeetCode
mathjax: true
---
本次周赛做出来两题，第三题栈溢出，数学规律没找对。
### [1678. 设计 Goal 解析器](https://leetcode-cn.com/problems/goal-parser-interpretation/)

请你设计一个可以解释字符串 command 的 Goal 解析器 。command 由 "G"、"()" 和/或 "(al)" 按某种顺序组成。Goal 解析器会将 "G" 解释为字符串 "G"、"()" 解释为字符串 "o" ，"(al)" 解释为字符串 "al" 。然后，按原顺序将经解释得到的字符串连接成一个字符串。

给你字符串 command ，返回 Goal 解析器 对 command 的解释结果。

<!--more-->
示例1：
```
输入：command = "G()(al)"
输出："Goal"
解释：Goal 解析器解释命令的步骤如下所示：
G -> G
() -> o
(al) -> al
最后连接得到的结果是 "Goal"
```

示例2:
```
输入：command = "G()()()()(al)"
输出："Gooooal"
```

示例3：
```
输入：command = "(al)G(al)()()G"
输出："alGalooG"
```

提示：
- 1 <= command.length <= 100
- command 由 "G"、"()" 和/或 "(al)" 按某种顺序组成

### 思路

直接字符串暴力模拟

### 代码
```c++
class Solution {
public:
    string interpret(string command) {
        string res;
        int n = command.size();
        int i = 0;
        while(i < n) {
            if (command[i] == 'G') {
                res += "G";
            } else if (i + 1 < n && command[i] == '(' && command[i+1] == ')') {
                res += "o";
            } else if (i + 3 < n && command[i] == '(' && command[i+1] == 'a' && command[i+2] == 'l' && command[i+3] == ')') {
                res += "al";
                i+=3;
            }
            i++;
        }
        return res;
    }
};
```

### [1679. K 和数对的最大数目](https://leetcode-cn.com/problems/max-number-of-k-sum-pairs/)

给你一个整数数组 nums 和一个整数 k 。

每一步操作中，你需要从数组中选出和为 k 的两个整数，并将它们移出数组。

返回你可以对数组执行的最大操作数。

示例 1：
```
输入：nums = [1,2,3,4], k = 5
输出：2
解释：开始时 nums = [1,2,3,4]：
- 移出 1 和 4 ，之后 nums = [2,3]
- 移出 2 和 3 ，之后 nums = []
不再有和为 5 的数对，因此最多执行 2 次操作。
```

示例 2：
```
输入：nums = [3,1,3,4,3], k = 6
输出：1
解释：开始时 nums = [3,1,3,4,3]：
- 移出前两个 3 ，之后nums = [1,4,3]
不再有和为 6 的数对，因此最多执行 1 次操作。
```

提示：
- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^9
- 1 <= k <= 10^9

### 思路
排序之后双指针
### 代码
```c++
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int i = 0 , j = n - 1;
        int res = 0;
        while (i < j) {
            if (nums[i] + nums[j] == k) {
                res++;
                i++;
                j--;
            } else if (nums[i] + nums[j] < k) {
                i++;
            } else if (nums[i] + nums[j] > k) {
                j--;
            }
        }
        return res;
    }
};
```

### [1680. 连接连续二进制数字](https://leetcode-cn.com/problems/concatenation-of-consecutive-binary-numbers/)

给你一个整数 n ，请你将 1 到 n 的二进制表示连接起来，并返回连接结果对应的 十进制 数字对 10^9 + 7 取余的结果。

示例 1：
```
输入：n = 1
输出：1
解释：二进制的 "1" 对应着十进制的 1 。
```

示例 2：
```
输入：n = 3
输出：27
解释：二进制下，1，2 和 3 分别对应 "1" ，"10" 和 "11" 。
将它们依次连接，我们得到 "11011" ，对应着十进制的 27 。
```

示例 3:
```
输入：n = 12
输出：505379714
解释：连接结果为 "1101110010111011110001001101010111100" 。
对应的十进制数字为 118505380540 。
对 109 + 7 取余后，结果为 505379714 。
```

提示：
- 1 <= n <= 10^5

### 思路

由于我们需要将「十进制转换成二进制」「进行运算」「将结果转换回十进制」这三个步骤，因此我们不妨直接将整个问题在十进制的角度下进行考虑。

假设我们当前处理到了数字 iii，并且前面 [1,i−1]的二进制连接起来对应的十进制数为 x，那么我们如何将数字 i 进行连接呢？

观察二进制连接的过程，我们可以将这一步运算抽象为两个步骤：

- 第一步会将之前 [1,i−1]的二进制数左移若干位，这个位数就是 i 的二进制表示的位数；

- 第二步将 i 通过加法运算与左移的结果进行相加。

将上面所有的运算转换为 10 进制，我们就可以得到 x 的递推式，即：

$$
 x=x \cdot 2^{\text {len} 2(i)}+i 
$$

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
[leetcode题解](https://leetcode-cn.com/problems/concatenation-of-consecutive-binary-numbers/solution/lian-jie-lian-xu-er-jin-zhi-shu-zi-by-ze-t40j/)
```c++
class Solution {
private:
    static constexpr int mod = 1000000007;
    
public:
    int concatenatedBinary(int n) {
        // 
        int ans = 0;
        int shift = 0;
        for (int i = 1; i <= n; ++i) {
            // if (131072 % i == 0) {
            if (!(i & (i - 1))) {
                ++shift;
            }
            ans = ((static_cast<long long>(ans) << shift) + i) % mod;
        }
        return ans;
    }
};
```

### [1681. 最小不兼容性](https://leetcode-cn.com/problems/minimum-incompatibility/)

给你一个整数数组 nums​​​ 和一个整数 k 。你需要将这个数组划分到 k 个相同大小的子集中，使得同一个子集里面没有两个相同的元素。

一个子集的 不兼容性 是该子集里面最大值和最小值的差。

请你返回将数组分成 k 个子集后，各子集 不兼容性 的 和 的 最小值 ，如果无法分成分成 k 个子集，返回 -1 。

子集的定义是数组中一些数字的集合，对数字顺序没有要求。

示例 1：
```
输入：nums = [1,2,1,4], k = 2
输出：4
解释：最优的分配是 [1,2] 和 [1,4] 。
不兼容性和为 (2-1) + (4-1) = 4 。
注意到 [1,1] 和 [2,4] 可以得到更小的和，但是第一个集合有 2 个相同的元素，所以不可行。
```

示例 2：
```
输入：nums = [6,3,8,1,3,1,2,2], k = 4
输出：6
解释：最优的子集分配为 [1,2]，[2,3]，[6,8] 和 [1,3] 。
不兼容性和为 (2-1) + (3-2) + (8-6) + (3-1) = 6 。
```

示例3：
```
输入：nums = [5,3,3,6,3,3], k = 3
输出：-1
解释：没办法将这些数字分配到 3 个子集且满足每个子集里没有相同数字。
```
提示：
- 1 <= k <= nums.length <= 16
- nums.length 能被 k 整除。
- 1 <= nums[i] <= nums.length

### 思路
此题是状态压缩dp。参考[题解](https://leetcode-cn.com/problems/minimum-incompatibility/solution/zui-xiao-bu-jian-rong-xing-by-zerotrac2-rwje/)

### 代码
```c++
class Solution {
public:
    int minimumIncompatibility(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> value(1 << n, -1);
        vector<int> freq(n + 1);
        for (int sub = 0; sub < (1 << n); ++sub) {
            // 判断 sub 是否有 n/k 个 1
            if (__builtin_popcount(sub) == n / k) {
                // 使用数组进行计数
                for (int j = 0; j < n; ++j) {
                    if (sub & (1 << j)) {
                        ++freq[nums[j]];
                    }
                }
                // 任意一个数不能出现超过 1 次
                bool flag = true;
                for (int j = 1; j <= n; ++j) {
                    if (freq[j] > 1) {
                        flag = false;
                        break;
                    }
                }
                // 如果满足要求，那么计算 sub 的不兼容性
                if (flag) {
                    int lb = INT_MAX, rb = INT_MIN;
                    for (int j = 1; j <= n; ++j) {
                        if (freq[j] > 0) {
                            lb = min(lb, j);
                            rb = max(rb, j);
                        }
                    }
                    value[sub] = rb - lb;
                }
                // 由于我们使用数组进行计数，因此要将数组恢复原状
                for (int j = 0; j < n; ++j) {
                    if (sub & (1 << j)) {
                        --freq[nums[j]];
                    }
                }
            }
        }
        
        vector<int> f(1 << n, -1);
        f[0] = 0;
        for (int mask = 1; mask < (1 << n); ++mask) {
            // 判断 mask 是否有 n/k 倍数个 1
            if (__builtin_popcount(mask) % (n / k) == 0) {
                // 枚举子集
                for (int sub = mask; sub; sub = (sub - 1) & mask) {
                    if (value[sub] != -1 && f[mask ^ sub] != -1) {
                        if (f[mask] == -1) {
                            f[mask] = f[mask ^ sub] + value[sub];
                        }
                        else {
                            f[mask] = min(f[mask], f[mask ^ sub] + value[sub]);
                        }
                    }
                }
            }
        }
            
        return f[(1 << n) - 1];
    }
};
```