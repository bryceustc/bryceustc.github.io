---
title: 【Leetcode】第258场周赛
date: 2021-09-12 11:55:30
tags: LeetCode
categories: LeetCode
---

### [5867. 反转单词前缀](https://leetcode-cn.com/contest/weekly-contest-258/problems/reverse-prefix-of-word/)

给你一个下标从 0 开始的字符串 word 和一个字符 ch 。找出 ch 第一次出现的下标 i ，反转 word 中从下标 0 开始、直到下标 i 结束（含下标 i ）的那段字符。如果 word 中不存在字符 ch ，则无需进行任何操作。

例如，如果 word = "abcdefd" 且 ch = "d" ，那么你应该 反转 从下标 0 开始、直到下标 3 结束（含下标 3 ）。结果字符串将会是 "dcbaefd" 。

返回 结果字符串 。

<!--more-->
示例1：
```
输入：word = "abcdefd", ch = "d"
输出："dcbaefd"
解释："d" 第一次出现在下标 3 。 
反转从下标 0 到下标 3（含下标 3）的这段字符，结果字符串是 "dcbaefd" 。
```

示例2:
```
输入：word = "xyxzxe", ch = "z"
输出："zxyxxe"
解释："z" 第一次也是唯一一次出现是在下标 3 。
反转从下标 0 到下标 3（含下标 3）的这段字符，结果字符串是 "zxyxxe" 。
```

示例3:
```
输入：word = "abcd", ch = "z"
输出："abcd"
解释："z" 不存在于 word 中。
无需执行反转操作，结果字符串是 "abcd" 。
```

提示：
- 1 <= word.length <= 250
- word 由小写英文字母组成
- ch 是一个小写英文字母

### 思路
暴力模拟。

### 代码
```c++
class Solution {
public:
    string reversePrefix(string word, char ch) {
        string res;
        int n = word.size();
        for (int i = 0; i < n; i++) {
            if (word[i] == ch) {
                for (int j = i; j >=0; j--) {
                    res += word[j];
                }
                for (int k = i+1; k < n; k++) {
                    res += word[k];
                }
                break;
            } else if (i == n - 1) {
                res = word;
                break;
            }
        }
        return res;
    }
};
// 大佬简洁做法
class Solution {
public:
    string reversePrefix(string s, char a) {
        int pos = find(s.begin(), s.end(), a) - s.begin();
        if(pos < s.size()) {
            reverse(s.begin(), s.begin() + pos + 1);
        }
        return s;
    }
};
```

### [5868. 可互换矩形的组数](https://leetcode-cn.com/contest/weekly-contest-258/problems/number-of-pairs-of-interchangeable-rectangles/)

用一个下标从 0 开始的二维整数数组 rectangles 来表示 n 个矩形，其中 rectangles[i] = [widthi, heighti] 表示第 i 个矩形的宽度和高度。

如果两个矩形 i 和 j（i < j）的宽高比相同，则认为这两个矩形 可互换 。更规范的说法是，两个矩形满足 $width_i/height_i == width_j/height_j$（使用实数除法而非整数除法），则认为这两个矩形 可互换 。

计算并返回 rectangles 中有多少对 可互换 矩形。

示例 1：
```
输入：rectangles = [[4,8],[3,6],[10,20],[15,30]]
输出：6
解释：下面按下标（从 0 开始）列出可互换矩形的配对情况：
- 矩形 0 和矩形 1 ：4/8 == 3/6
- 矩形 0 和矩形 2 ：4/8 == 10/20
- 矩形 0 和矩形 3 ：4/8 == 15/30
- 矩形 1 和矩形 2 ：3/6 == 10/20
- 矩形 1 和矩形 3 ：3/6 == 15/30
- 矩形 2 和矩形 3 ：10/20 == 15/30
```

示例 2：
```
输入：rectangles = [[4,5],[7,8]]
输出：0
解释：不存在成对的可互换矩形。
```

提示：
- n == rectangles.length
- 1 <= n <= 10^5
- rectangles[i].length == 2
- 1 <= widthi, heighti <= 10^5

### 思路
统计数组中宽高比相同的个数，然后求一个排列组合C(n,2);
### 代码
```c++
class Solution {
public:
    long long interchangeableRectangles(vector<vector<int>>& rectangles) {
        long long res = 0;
        int n = rectangles.size();
        unordered_map<double, unsigned long long> m;
        for (int i = 0; i < n; i++) {
            double a = rectangles[i][0] + 0.0;
            double b = rectangles[i][1] + 0.0;
            m[a/b]++; 
        }
        for (auto &p : m) {
            if (p.second > 1) {
                res += combo(p.second);
            }
        }
        return res;
    }

    unsigned long long combo(unsigned long long n)
    {
        unsigned long long com = n * (n - 1) / 2;
        return com;
    }
};
```

### [5869. 两个回文子序列长度的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/)

给你一个字符串 s ，请你找到 s 中两个 不相交回文子序列 ，使得它们长度的 乘积最大 。两个子序列在原字符串中如果没有任何相同下标的字符，则它们是 不相交 的。

请你返回两个回文子序列长度可以达到的**最大乘积**。

子序列 指的是从原字符串中删除若干个字符（可以一个也不删除）后，剩余字符不改变顺序而得到的结果。如果一个字符串从前往后读和从后往前读一模一样，那么这个字符串是一个 回文字符串 。

示例 1：

![](https://assets.leetcode.com/uploads/2021/08/24/two-palindromic-subsequences.png)
```
输入：s = "leetcodecom"
输出：9
解释：最优方案是选择 "ete" 作为第一个子序列，"cdc" 作为第二个子序列。
它们的乘积为 3 * 3 = 9 。
```

示例 2：
```
输入：s = "bb"
输出：1
解释：最优方案为选择 "b" （第一个字符）作为第一个子序列，"b" （第二个字符）作为第二个子序列。
它们的乘积为 1 * 1 = 1 。
```
示例 3：
```
输入：s = "accbcaxxcxx"
输出：25
解释：最优方案为选择 "accca" 作为第一个子序列，"xxcxx" 作为第二个子序列。
它们的乘积为 5 * 5 = 25 。
```


提示：
- 2 <= s.length <= 12
- s 只含有小写英文字母。

### 思路

注意此题的数据量比较小，可以直接暴力遍历所有的子序列进行判断，但是注意使用暴力时，需要考虑引用，减少消耗，否则会超时。

### 代码
```c++
class Solution {
public:
    int res = 0;
    int maxProduct(string s) {
        string s1, s2;
        dfs(s, s1, s2, 0);
        return res;
    }
    void dfs(string &s, string s1, string s2, int idx) {
        if (check(s1) && check(s2)) {
            res = max(res, int(s1.size() * s2.size()));
        }
        if (idx == s.size()) return;
        dfs(s, s1 + s[idx], s2, idx + 1); //子序列s1使用该字符
        dfs(s, s1, s2 + s[idx], idx + 1); //子序列s2使用该字符
        dfs(s, s1, s2, idx + 1);//子序列都不使用该字符
    }
    bool check(string &s) {
        int l = 0, r = s.size() - 1;
        while(l < r) {
            if (s[l++] != s[r--]) return false; 
        }
        return true;
    }
};
```

### [1998. 数组的最大公因数排序](https://leetcode-cn.com/problems/gcd-sort-of-an-array/)

有一棵根节点为 0 的 家族树 ，总共包含 n 个节点，节点编号为 0 到 n - 1 。给你一个下标从 0 开始的整数数组 parents ，其中 parents[i] 是节点 i 的父节点。由于节点 0 是 根 ，所以 parents[0] == -1 。

总共有 105 个基因值，每个基因值都用 闭区间 [1, 105] 中的一个整数表示。给你一个下标从 0 开始的整数数组 nums ，其中 nums[i] 是节点 i 的基因值，且基因值 互不相同 。

请你返回一个数组 ans ，长度为 n ，其中 ans[i] 是以节点 i 为根的子树内 缺失 的 最小 基因值。

节点 x 为根的 子树 包含节点 x 和它所有的 后代 节点。

示例 1：

![](https://assets.leetcode.com/uploads/2021/08/23/case-1.png)

```
输入：parents = [-1,0,0,2], nums = [1,2,3,4]
输出：[5,1,1,1]
解释：每个子树答案计算结果如下：
- 0：子树包含节点 [0,1,2,3] ，基因值分别为 [1,2,3,4] 。5 是缺失的最小基因值。
- 1：子树只包含节点 1 ，基因值为 2 。1 是缺失的最小基因值。
- 2：子树包含节点 [2,3] ，基因值分别为 [3,4] 。1 是缺失的最小基因值。
- 3：子树只包含节点 3 ，基因值为 4 。1是缺失的最小基因值。
```

示例 2：

![](https://assets.leetcode.com/uploads/2021/08/23/case-2.png)

```
输入：parents = [-1,0,1,0,3,3], nums = [5,4,6,2,1,3]
输出：[7,1,1,4,2,1]
解释：每个子树答案计算结果如下：
- 0：子树内包含节点 [0,1,2,3,4,5] ，基因值分别为 [5,4,6,2,1,3] 。7 是缺失的最小基因值。
- 1：子树内包含节点 [1,2] ，基因值分别为 [4,6] 。 1 是缺失的最小基因值。
- 2：子树内只包含节点 2 ，基因值为 6 。1 是缺失的最小基因值。
- 3：子树内包含节点 [3,4,5] ，基因值分别为 [2,1,3] 。4 是缺失的最小基因值。
- 4：子树内只包含节点 4 ，基因值为 1 。2 是缺失的最小基因值。
- 5：子树内只包含节点 5 ，基因值为 3 。1 是缺失的最小基因值。
```

示例3：
```
输入：parents = [-1,2,3,0,2,4,1], nums = [2,3,4,5,6,7,8]
输出：[1,1,1,1,1,1,1]
```

提示：
- n == parents.length == nums.length
- 2 <= n <= 10^5
- 对于 i != 0 ，满足 0 <= parents[i] <= n - 1
- parents[0] == -1
- parents 表示一棵合法的树。
- 1 <= nums[i] <= 10^5
- nums[i] 互不相同。

### 思路
启发式合并

### 代码
```c++
class Solution {
    vector<int> e[100010], ans;

    int now = 1;
    bool has1[100010], vis[100010];

    void dfs1(int sn, const vector<int> &nums) {
        if (nums[sn] == 1) has1[sn] = true;
        else has1[sn] = false;
        for (int fn : e[sn]) {
            dfs1(fn, nums);
            has1[sn] |= has1[fn];
        }
        if (!has1[sn]) ans[sn] = 1;
    }

    void dfs3(int sn, const vector<int> &nums) {
        for (int fn : e[sn]) dfs3(fn, nums);
        vis[nums[sn]] = true;
    }

    void dfs2(int sn, const vector<int> &nums) {
        if (!has1[sn]) return;
        for (int fn : e[sn]) if (has1[fn]) dfs2(fn, nums);
        // 记录没有 1 的分支的数
        for (int fn : e[sn]) if (!has1[fn]) dfs3(fn, nums);
        vis[nums[sn]] = true;
        // 计算本节点的答案
        while (vis[now]) now++;
        ans[sn] = now;
    }

public:
    vector<int> smallestMissingValueSubtree(vector<int>& parents, vector<int>& nums) {
        int n = parents.size();
        ans = vector<int>(n);
        for (int i = 1; i < parents.size(); i++) e[parents[i]].push_back(i);
        // 第一遍 dfs，先把哪些子树里有 1 找出来
        dfs1(0, nums);
        // 第二遍 dfs，计算有 1 的子树的答案
        dfs2(0, nums);
        return ans;
    }
};
```