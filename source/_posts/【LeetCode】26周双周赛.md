---
title: 【LeetCode】26周双周赛
date: 2020-05-17 10:55:23
tags: LeetCode
categories: LeetCode
---
本周题目AC 3/4，第四较为复杂，思路不是很清晰。

### [5396. 连续字符](https://leetcode-cn.com/problems/consecutive-characters/)

给你一个字符串 s ，字符串的「能量」定义为：只包含一种字符的最长非空子字符串的长度。

请你返回字符串的能量。
<!--more-->

示例1：
```
输入：s = "leetcode"
输出：2
解释：子字符串 "ee" 长度为 2 ，只包含字符 'e' 。
```

示例2:
```
输入：s = "abbcccddddeeeeedcba"
输出：5
解释：子字符串 "eeeee" 长度为 5 ，只包含字符 'e' 。
```

示例3：
```
输入：s = "hooraaaaaaaaaaay"
输出：11
```

### 思路

直接模拟暴力即可，此题竞赛的时候，考虑复杂了，错了两次被罚时。

跟前一个比较，相等的话，计数+1，不等，计数置1
记录最大计数

### 代码
```c++
class Solution {
public:
    int maxPower(string s) {
        int res =0 ;
        int n = s.size();
        int count = 1;
        for (int i=1;i<n;i++)
        {
            if (s[i]==s[i-1])
            {
                count++;
            }
            else
            {
                res = max(res,count);
                count=1;
            }
        }
        res = max(res,count);
        return res;
    }
};
```

### [5397. 最简分数](https://leetcode-cn.com/problems/check-if-all-1s-are-at-least-length-k-places-away/)

给你一个整数 n ，请你返回所有 0 到 1 之间（不包括 0 和 1）满足分母小于等于  n 的 最简 分数 。分数可以以 任意 顺序返回。

示例 1：


```
输入：n = 2
输出：["1/2"]
解释："1/2" 是唯一一个分母小于等于 2 的最简分数。
```

示例 2：
```
输入：n = 3
输出：["1/2","1/3","2/3"]
```
示例 3：
```
输入：n = 1
输出：[]
```

示例4：
```
输入：n = 4
输出：["1/2","1/3","1/4","2/3","3/4"]
解释："2/4" 不是最简分数，因为它可以化简为 "1/2" 。
```

提示：
- 1 <= n <= 10

### 思路
求最大公约数，化简分子分母，遍历所有可能，利用哈希来去重，

或者 不用哈希来去重，找最大公约数为1的情况
### 代码
hash去重
```
class Solution {
public:
    vector<string> simplifiedFractions(int n) {
        vector<string> res;
        unordered_set<string> s;
        if (n==1) return res;
        for (int i=2;i<=n;i++)
        {
            for (int j=1;j<i;j++)
            {
                int g = __gcd(i,j);
                int x = i/g;
                int y = j/g;
                string str = to_string(y) + "/" + to_string(x);
                s.insert(str);
            }
        }
        for (auto &a: s)
        {
            res.push_back(a);
        }
        return res;
    }
};
```
最大公约数为1的情况
```c++
class Solution {
public:
    vector<string> simplifiedFractions(int n) {
        vector<string> res;
        if (n==1) return res;
        for (int i=2;i<=n;++i)
        {
            for (int j=1;j<i;++j)
            {
                int g = __gcd (i,j);
                if (g==1)   // 公约数为1说明是最简了
                {
                    string s = to_string(j) +"/" + to_string(i);
                    res.push_back(s);
                }
            }
        }
        return res;
    }
};
```
### [5398. 统计二叉树中好节点的数目](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

给你一棵根为 root 的二叉树，请你返回二叉树中好节点的数目。

「好节点」X 定义为：从根到该节点 X 所经过的节点中，没有任何节点的值大于 X 的值。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/16/test_sample_1.png)
```
输入：root = [3,1,4,3,null,1,5]
输出：4
解释：图中蓝色节点为好节点。
根节点 (3) 永远是个好节点。
节点 4 -> (3,4) 是路径中的最大值。
节点 5 -> (3,4,5) 是路径中的最大值。
节点 3 -> (3,1,3) 是路径中的最大值。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/16/test_sample_2.png)
```
输入：root = [3,3,null,4,2]
输出：3
解释：节点 2 -> (3, 3, 2) 不是好节点，因为 "3" 比它大。
```
示例 3：
```
输入：root = [1]
输出：1
解释：根节点是好节点。
```

提示：
- 二叉树中节点数目范围是 [1, 10^5] 。
- 每个节点权值的范围是 [-10^4, 10^4] 。

### 思路

DFS

DFS时每次传输从根节点到该节点路劲上的最大值，判断当前的节点的值是否大于等于路径的最大值即可。

### 代码

multiset
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int goodNodes(TreeNode* root) {
        if (root==nullptr) return 0;
        int res = 0;
        dfs(root,res,INT_MIN);
        return res;
    }
    void dfs(TreeNode* root, int &res, int max_val)
    {
        if (root==nullptr) return;
        if(root->val>=max_val)
        {
            res++;
        }
        max_val =max(root->val,max_val);
        dfs(root->left,res,max_val);
        dfs(root->right,res,max_val);
    }
};
```

### [5399. 数位成本和为目标值的最大数字](https://leetcode-cn.com/problems/form-largest-integer-with-digits-that-add-up-to-target/)

给你一个整数数组 cost 和一个整数 target 。请你返回满足如下规则可以得到的 最大 整数：

- 给当前结果添加一个数位（i + 1）的成本为 cost[i] （cost 数组下标从 0 开始）。
- 总成本必须恰好等于 target 。
- 添加的数位中没有数字 0 。


由于答案可能会很大，请你以字符串形式返回。

如果按照上述要求无法得到任何整数，请你返回 "0" 。

示例 1：

```
输入：cost = [4,3,2,5,6,7,2,5,5], target = 9
输出："7772"
解释：添加数位 '7' 的成本为 2 ，添加数位 '2' 的成本为 3 。所以 "7772" 的代价为 2*3+ 3*1 = 9 。 "997" 也是满足要求的数字，但 "7772" 是较大的数字。
 数字     成本
  1  ->   4
  2  ->   3
  3  ->   2
  4  ->   5
  5  ->   6
  6  ->   7
  7  ->   2
  8  ->   5
  9  ->   5
```

示例 2：

```
输入：cost = [7,6,5,5,5,6,8,7,8], target = 12
输出："85"
解释：添加数位 '8' 的成本是 7 ，添加数位 '5' 的成本是 5 。"85" 的成本为 7 + 5 = 12 。
```
示例 3：
```
输入：cost = [2,4,6,2,4,6,4,4,4], target = 5
输出："0"
解释：总成本是 target 的条件下，无法生成任何整数。
```

示例4：
```
输入：cost = [6,10,15,40,40,40,40,40,40], target = 47
输出："32211"
```

提示：
- cost.length == 9
- 1 <= cost[i] <= 5000
- 1 <= target <= 5000

### 思路
参考[黄歪歪](https://www.bilibili.com/video/BV1JK4y1t7nu?p=5)，dp + 贪心，此题与322题零钱兑换有点类似

### 代码

```c++
class Solution {
public:
    string largestNumber(vector<int>& cost, int target) {
        string res;
        vector<int> dp(target+1,INT_MIN); 
        dp[0]=0;
        for(int i=1;i<=target;++i)
        {
            for (int j=1;j<=9;++j)
            {
                int c = cost[j-1];
                if (i>=c)
                {
                    dp[i]=max(dp[i],dp[i-c]+1); // 求出能组成target的最大位数，零钱兑换是取最小
                }
            }
        }
        if (dp[target]<0) return "0";
        // 取结果，按照贪心来求
        int now = target;
        while(now>0)
        {
            for (int i=9;i>=1;--i)
            {
                int c = cost[i-1];
                if (now>=c && dp[now]==dp[now-c]+1)
                {
                    res += to_string(i);
                    now-=c;
                    break;
                }
            }
        }
        return res;
    }
};
```