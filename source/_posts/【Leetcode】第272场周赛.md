---
title: 【Leetcode】第272场周赛
date: 2021-12-19 23:08:40
tags: LeetCode
categories: LeetCode
---

### [5956. 找出数组中的第一个回文字符串](https://leetcode-cn.com/problems/find-first-palindromic-string-in-the-array/)

给你一个字符串数组 words ，找出并返回数组中的 第一个回文字符串 。如果不存在满足要求的字符串，返回一个 空字符串 "" 。

回文字符串 的定义为：如果一个字符串正着读和反着读一样，那么该字符串就是一个 回文字符串 。

<!--more-->
示例1：
```
输入：words = ["abc","car","ada","racecar","cool"]
输出："ada"
解释：第一个回文字符串是 "ada" 。
注意，"racecar" 也是回文字符串，但它不是第一个。
```

示例2:
```
输入：words = ["notapalindrome","racecar"]
输出："racecar"
解释：第一个也是唯一一个回文字符串是 "racecar" 。
```

示例3:
```
输入：words = ["def","ghi"]
输出：""
解释：不存在回文字符串，所以返回一个空字符串。
```

提示：
- 1 <= words.length <= 100
- 1 <= words[i].length <= 100
- words[i] 仅由小写英文字母组成

### 思路
直接暴力模拟，回文字符串的判断，需要再更熟练一点哦。
### 代码
```c++
class Solution {
public:
    string firstPalindrome(vector<string>& words) {
        int n = words.size();
        string res;
        for (int i = 0; i < n; i++) {
            string s1 = words[i];
            string s2 = words[i];
            reverse(s2.begin(),s2.end());
            if (s2 == s1) {
                return s1;
            }
        }
        return res;
    }
};
```

### [5957. 向字符串添加空格](https://leetcode-cn.com/problems/adding-spaces-to-a-string/)

给你一个下标从 0 开始的字符串 s ，以及一个下标从 0 开始的整数数组 spaces 。

数组 spaces 描述原字符串中需要添加空格的下标。每个空格都应该插入到给定索引处的字符值 之前 。

- 例如，s = "EnjoyYourCoffee" 且 spaces = [5, 9] ，那么我们需要在 'Y' 和 'C' 之前添加空格，这两个字符分别位于下标 5 和下标 9 。因此，最终得到 "Enjoy Your Coffee" 。

请你添加空格，并返回修改后的字符串。

示例 1：
```
输入：s = "LeetcodeHelpsMeLearn", spaces = [8,13,15]
输出："Leetcode Helps Me Learn"
解释：
下标 8、13 和 15 对应 "LeetcodeHelpsMeLearn" 中加粗斜体字符。
接着在这些字符前添加空格。
```

示例 2：
```
输入：s = "icodeinpython", spaces = [1,5,7,9]
输出："i code in py thon"
解释：
下标 1、5、7 和 9 对应 "icodeinpython" 中加粗斜体字符。
接着在这些字符前添加空格。
```

示例 3：
```
输入：s = "spacing", spaces = [0,1,2,3,4,5,6]
输出：" s p a c i n g"
解释：
字符串的第一个字符前可以添加空格。
```

提示：
- 1 <= s.length <= 3 * 105
- s 仅由大小写英文字母组成
- 1 <= spaces.length <= 3 * 105
- 0 <= spaces[i] <= s.length - 1
- spaces 中的所有值 严格递增

### 思路
双指针,我们可以使用两个指针分别遍历字符串 s 和数组 spaces。记前者的指针为 i，后者的指针为 ptr：当 spaces[ptr] 恰好与 i 相等时，我们在答案字符串的末尾放入一个空格，并将 ptr 向右移动一个位置。

此外，我们还需要在答案字符串的末尾放入 s[i]，并将 ii 向右移动一个位置。在两个指针全部遍历完成后，我们就得到了修改后的字符串。

### 代码
```c++
class Solution {
public:
    string addSpaces(string s, vector<int>& spaces) {
        int n = s.size();
        string ans;
        ans.reserve(n + spaces.size());
        
        int ptr = 0;
        for (int i = 0; i < n; ++i) {
            if (ptr < spaces.size() && spaces[ptr] == i) {
                ans.push_back(' ');
                ++ptr;
            }
            ans.push_back(s[i]);
        }
        return ans;
    }
};
```

### [5958. 股票平滑下跌阶段的数目](https://leetcode-cn.com/problems/number-of-smooth-descent-periods-of-a-stock/)

给你一个整数数组prices，表示一支股票的历史每日股价，其中prices[i]是这支股票第i天的价格。

一个 平滑下降的阶段定义为：对于连续一天或者多天，每日股价都比 前一日股价恰好少 1，这个阶段第一天的股价没有限制。

请你返回平滑下降阶段的数目。

示例 1：
```
输入：prices = [3,2,1,4]
输出：7
解释：总共有 7 个平滑下降阶段：
[3], [2], [1], [4], [3,2], [2,1] 和 [3,2,1]
注意，仅一天按照定义也是平滑下降阶段。
```

示例 2：
```
输入：prices = [8,6,7,7]
输出：4
解释：总共有 4 个连续平滑下降阶段：[8], [6], [7] 和 [7]
由于 8 - 6 ≠ 1 ，所以 [8,6] 不是平滑下降阶段。
```
示例 3：
```
输入：prices = [1]
输出：1
解释：总共有 1 个平滑下降阶段：[1]
```

提示：
- 1 <= prices.length <= 10^5
- 1 <= prices[i] <= 10^5

### 思路

双指针按照题意模拟即可

### 代码
```c++
class Solution {
public:
    long long getDescentPeriods(vector<int>& prices) {
        long long res = 0;
        int n = prices.size();
        int i = 0;
        while (i < n) {
            int j = i + 1;
            while (j < n && prices[j-1] - prices[j] == 1) {
                j++;
            }
            res += (long long)(j - i) * (long long)(j - i + 1) / 2LL;
            i = j;
        }
        return res;
    }
};
```

### [5959. 使数组 K 递增的最少操作次数](https://leetcode-cn.com/problems/minimum-operations-to-make-the-array-k-increasing/)

给你一个下标从 0 开始包含 n 个正整数的数组 arr ，和一个正整数 k 。

如果对于每个满足 k <= i <= n-1 的下标 i ，都有 arr[i-k] <= arr[i] ，那么我们称 arr 是 K 递增 的。

- 比方说，arr = [4, 1, 5, 2, 6, 2] 对于 k = 2 是 K 递增的，因为：
    - arr[0] <= arr[2] (4 <= 5)
    - arr[1] <= arr[3] (1 <= 2)
    - arr[2] <= arr[4] (5 <= 6)
    - arr[3] <= arr[5] (2 <= 2)

- 但是，相同的数组 arr 对于 k = 1 不是 K 递增的（因为 arr[0] > arr[1]），对于 k = 3 也不是 K 递增的（因为 arr[0] > arr[3] ）。
每一次 操作 中，你可以选择一个下标 i 并将 arr[i] 改成任意 正整数。

请你返回对于给定的 k ，使数组变成 K 递增的 最少操作次数 。

示例 1：
```
输入：arr = [5,4,3,2,1], k = 1
输出：4
解释：
对于 k = 1 ，数组最终必须变成非递减的。
可行的 K 递增结果数组为 [5,6,7,8,9]，[1,1,1,1,1]，[2,2,3,4,4] 。它们都需要 4 次操作。
次优解是将数组变成比方说 [6,7,8,9,10] ，因为需要 5 次操作。
显然我们无法使用少于 4 次操作将数组变成 K 递增的。
```

示例 2：

```
输入：arr = [4,1,5,2,6,2], k = 2
输出：0
解释：
这是题目描述中的例子。
对于每个满足 2 <= i <= 5 的下标 i ，有 arr[i-2] <= arr[i] 。
由于给定数组已经是 K 递增的，我们不需要进行任何操作。
```

示例3：
```
输入：arr = [4,1,5,2,6,2], k = 3
输出：2
解释：
下标 3 和 5 是仅有的 3 <= i <= 5 且不满足 arr[i-3] <= arr[i] 的下标。
将数组变成 K 递增的方法之一是将 arr[3] 变为 4 ，且将 arr[5] 变成 5 。
数组变为 [4,1,5,4,6,5] 。
可能有其他方法将数组变为 K 递增的，但没有任何一种方法需要的操作次数小于 2 次。
```

提示：
- 1 <= arr.length <= 10^5
- 1 <= arr[i], k <= arr.length

### 思路
类似最长递增子序列

### 代码
```c++
class Solution {
public:
  int kIncreasing(vector<int>& arr, int k) {
    int n = arr.size();
    int ans = 0;
    int i = 0;
    while (i < k) {
      // 求出子序列
      vector<int> cur;
      int j = i;
      while (j < n) {
        cur.push_back(arr[j]);
        j += k;
      }
      // 求出操作数
      ans += cur.size() - getCount(cur);
      i++;
    }
    return ans;
  }

  int getCount(vector<int>& arr) {
    int n = arr.size();
    vector<int> tail;
    tail.push_back(arr[0]);
    for (int i = 1; i < n; ++i) {
      // 非严格递增要有等号
      if (arr[i] >= tail.back()) tail.push_back(arr[i]);
      else {
        /* 求出arr[i]在tail中替换的位置
         * 严格递增，代表相同元素应该被替换，所以求 大于等于arr[i]的下界
         * 非严格的递增，代表可以接在相同元素后面，所以求 大于arr[i]的下界
         * e.g.
         * arr[i] = 1, tail = [1,2,3]
         * 替换后
         * 严格递增为 tail = [1,2,3]
         * 非严格的递增为 tail = [1,1,3]
         * */
        int left = 0, right = tail.size();
        while (left < right) {
          int mid = left + (right - left) / 2;
          if (tail[mid] > arr[i]) right = mid;
          else left = mid + 1;
        }
        tail[left] = arr[i];
      }
    }
    return tail.size();
  }
};
```