---
title: 【Leetcode】第190场周赛
date: 2020-05-24 15:31:07
tags: LeetCode
categories: LeetCode
---
## 第190场周赛

本周题目AC 2.7/4，这次做的很一般，周末赖床迟到了半小时，下次不要赖床了！！！有些模板比如字符串分割的套路，需要熟练掌握。


### [5416. 检查单词是否为句中其他单词的前缀](https://leetcode-cn.com/problems/check-if-a-word-occurs-as-a-prefix-of-any-word-in-a-sentence/)

给你一个字符串 sentence 作为句子并指定检索词为 searchWord ，其中句子由若干用 单个空格 分隔的单词组成。

请你检查检索词 searchWord 是否为句子 sentence 中任意单词的前缀。
<!--more-->

- 如果 searchWord 是某一个单词的前缀，则返回句子 sentence 中该单词所对应的下标（下标从 1 开始）。
- 如果 searchWord 是多个单词的前缀，则返回匹配的第一个单词的下标（最小下标）。
- 如果 searchWord 不是任何单词的前缀，则返回 -1 。

字符串 S 的 「前缀」是 S 的任何前导连续子字符串。
示例1：
```
输入：sentence = "i love eating burger", searchWord = "burg"
输出：4
解释："burg" 是 "burger" 的前缀，而 "burger" 是句子中第 4 个单词。
```

示例2:
```
输入：sentence = "this problem is an easy problem", searchWord = "pro"
输出：2
解释："pro" 是 "problem" 的前缀，而 "problem" 是句子中第 2 个也是第 6 个单词，但是应该返回最小下标 2 。
```

示例3：
```
输入：sentence = "i am tired", searchWord = "you"
输出：-1
解释："you" 不是句子中任何单词的前缀。
```

示例4：
```
输入：sentence = "i use triple pillow", searchWord = "pill"
输出：4
```

示例4：
```
输入：sentence = "hello from the other side", searchWord = "they"
输出：-1
```
提示：
- 1 <= sentence.length <= 100
- 1 <= searchWord.length <= 10
- sentence 由小写英文字母和空格组成。
- searchWord 由小写英文字母组成。
- 前缀就是紧密附着于词根的语素，中间不能插入其它成分，并且它的位置是固定的——-位于词根之前。
### 思路

遇到字符串分割的题，其实可以考虑用一下Python，此题的思路就是按照空格分割好单词之后，逐个判断就好，注意掌握string类型find函数的用法

### 代码
```c++
class Solution {
public:
    int isPrefixOfWord(string sentence, string searchWord) {
        int res = -1;
        string s = sentence;
        string t = searchWord;
        vector<string> temp;
        string str;
        for (int i=0;i<s.size();++i)
        {
            if (s[i]!=' ')
            {
                str+=s[i];
            }
            else
            {
                temp.push_back(str);
                str="";
            }
        }
        temp.push_back(str);
        // 利用stringstream进行字符串分割
        // stringstream  ss(s);
        // while (ss >> str) {
        //     temp.push_back(str);
        // } 


        int n = temp.size();
        for (int i=0;i<n;++i)
        {
            if (temp[i].find(t)!=temp[i].npos && temp[i].find_first_of(t)==0)
            {
                return i+1;
            }
        }
        return res;
    }
};
```
看到比较简洁的C++代码,利用stringstream，学习一下：
```c++
class Solution {
public:
    int isPrefixOfWord(string sentence, string searchWord) {
        stringstream ss(sentence);
        int nCount=0;
        while(1){
            nCount++;
            string word;
            ss>>word;
            if(ss.fail())break;
            if(word.find(searchWord)==0){
                return nCount;
            }
        }
        return -1;
    }
};
```

利用Python来求解：
```python
class Solution:
    def isPrefixOfWord(self, s: str, w: str) -> int:
        s = s.split()
        n = len(s)
        for i in range(n):
            if s[i].startswith(w):
                return i+1
        return -1
```
### [5417. 定长子串中元音的最大数目](https://leetcode-cn.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

给你字符串 s 和整数 k 。

请返回字符串 s 中长度为 k 的单个子字符串中可能包含的最大元音字母数。

英文中的 元音字母 为（a, e, i, o, u）。

示例 1：

```
输入：s = "abciiidef", k = 3
输出：3
解释：子字符串 "iii" 包含 3 个元音字母。
```

示例 2：
```
输入：s = "aeiou", k = 2
输出：2
解释：任意长度为 2 的子字符串都包含 2 个元音字母。
```
示例 3：
```
输入：s = "leetcode", k = 3
输出：2
解释："lee"、"eet" 和 "ode" 都包含 2 个元音字母。
```

示例4：
```
输入：s = "rhythms", k = 4
输出：0
解释：字符串 s 中不含任何元音字母。
```

示例5：
```
输入：s = "tryhard", k = 4
输出：1
```

提示：
- 1 <= s.length <= 10^5
- s 由小写英文字母组成
- 1 <= k <= s.length

### 思路
滑动窗口，窗口大小为K.判定窗口内的元音字母出现的次数。
### 代码
```
class Solution {
public:
    int maxVowels(string s, int k) {
        int n = s.size();
        int res = 0;
        int left = 0, right = 0;
        int count =0;
        while(right <n)
        {
            char c = s[right++];
            if (c=='a'||c=='e'||c=='i'||c=='o'||c=='u')
            {
                count++;
            }
            if (right - left < k)
            {
                continue;
            }
            while(right -left>k)
            {
                char d = s[left];
                left++;
                if (d=='a'||d=='e'||d=='i'||d=='o'||d=='u')
                {
                    count--;
                }
            }
            if ((right -left)==k)
            {
                res = max(res,count);
            }
        }
        return res;
    }
};
```

### [5418. 二叉树中的伪回文路径](https://leetcode-cn.com/problems/pseudo-palindromic-paths-in-a-binary-tree/)

给你一棵二叉树，每个节点的值为 1 到 9 。我们称二叉树中的一条路径是 「伪回文」的，当它满足：路径经过的所有节点值的排列中，存在一个回文序列。

请你返回从根到叶子节点的所有路径中 伪回文 路径的数目。

示例 1：
![](1.png)
```
输入：root = [2,3,1,3,1,null,1]
输出：2 
解释：上图为给定的二叉树。总共有 3 条从根到叶子的路径：红色路径 [2,3,3] ，绿色路径 [2,1,1] 和路径 [2,3,1] 。
     在这些路径中，只有红色和绿色的路径是伪回文路径，因为红色路径 [2,3,3] 存在回文排列 [3,2,3] ，绿色路径 [2,1,1] 存在回文排列 [1,2,1] 。
```

示例 2：
![](2.png)
```
输入：root = [2,1,1,1,3,null,null,null,null,null,1]
输出：1 
解释：上图为给定二叉树。总共有 3 条从根到叶子的路径：绿色路径 [2,1,1] ，路径 [2,1,3,1] 和路径 [2,1] 。
     这些路径中只有绿色路径是伪回文路径，因为 [2,1,1] 存在回文排列 [1,2,1] 。
```
示例 3：
```
输入：root = [9]
输出：1
```

提示：
- 给定二叉树的节点数目在 1 到 10^5 之间。
- 节点值在 1 到 9 之间。

### 思路

这个题有点可惜，下来检查发现，有一部分代码选定条件写错，下来再改就通过了。但是耗时和占用内存空间很大，需要优化。

主要思路是：看到节点数目限制为10的5次方，可以直接暴力遍历，列出所有的根节点到叶子节点的路径，然后判断是否是伪回文。

1. 我们通过DFS，统计每条从根节点到叶子节点的路径上的数字个数。
2. 如果路径上的数字有1个以上的数字的数量为奇数，则该路径不能组成回文数。



### 代码
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
    int pseudoPalindromicPaths (TreeNode* root) {
        vector<vector<int>> path;
        vector<int> out;
        if (root==NULL) return res;
        dfs(root,out,path);
        int n = path.size();
        for (int i=0;i<n;++i)
        {
            if (helper(path[i]))
            {
                res++;
            }
        }
        return res;
    }
    void dfs(TreeNode* root, vector<int> out, vector<vector<int>> &path)
    {
        if (root !=NULL) 
        {
            out.push_back(root->val);
            if(root->left ==NULL && root->right==NULL)
            {
                path.push_back(out);
                return;
            }
            dfs(root->left,out,path);
            dfs(root->right,out,path);
        }
    }
    bool helper(vector<int> temp)
    {
        unordered_map<int,int> m;
        int count = 0;
        for (int & a : temp)
        {
            m[a]++;
        }
        for (auto &p : m)
        {
            int a = p.first;
            if (m[a]&1)
            {
                count++;
            }
        }
        if (count > 1) return false;
        return true;
    }
private:
        int res =0;
};
```
优化：

利用位运算来求解，因为节点的值限制在1-9，充分利用这一点。

总所周知 n & (n - 1) 可以用来消除最后一个1

举例: 以下都是二进制表示

假设 n = 100100 那么 n - 1 = 100011 因为100 - 1 = 011

那么 n & (n - 1) = 100000 所以这是去掉了最后的一个1

因为异或的性质是 位相同异或为0 位不同异或为1 例如 1 ^ 1 = 0 ; 1 ^ 0 =1

然后比如 5 二进制为 1001 , 依据异或的性质 1001 ^ 1001 = 0

所以异或的性质就是两个相同的数 异或为0

然后回文串只会有两种情况 要么 1221 要么 121 所以异或后，要么是0 要么是个数为奇数的数字。

然后因为题目取值范围是1～9 所以可以用**每一位代表一个数字。**

比如5写成100000(1左移5位)，这样哪怕是121这种情况 异或之后也只会剩下2 也就是100

这样我们就可以根据 n & (n - 1) 来消除1 ，因为满足伪回文的只能有一个数值个数是奇数，也就是说只能有有一位为1，所以判断是否为回文数字的条件就变成了
```
(n == 0 || (n & (n - 1)) == 0)
```
根据优化之后写的简洁代码
```c++
class Solution {
public:

    int res = 0;
    int pseudoPalindromicPaths (TreeNode* root) {
        if(root==nullptr) return 0;
        int num = 0;
        dfs(root,num); // 深度优先遍历
        return res;
    }
    void dfs(TreeNode* root, int num)
    {
        if (root!=nullptr)
        {
            int n = num ^ (1<< root->val); // 节点的val为几就左移几位,代表这一位的状态
            if (root->left==nullptr && root->right==nullptr)//判断是否是叶子节点
            {
                if (n==0 || (n&(n-1))==0) //判断是否为伪回文
                {
                    res++;
                }
            }
            dfs(root->left,n);
            dfs(root->right,n);
        }
    }
};
```

### [5419. 两个子序列的最大点积](https://leetcode-cn.com/problems/max-dot-product-of-two-subsequences/)

给你两个数组 nums1 和 nums2 。

请你返回 nums1 和 nums2 中两个长度相同的 非空 子序列的最大点积。

数组的非空子序列是通过删除原数组中某些元素（可能一个也不删除）后剩余数字组成的序列，但不能改变数字间相对顺序。比方说，[2,3,5] 是 [1,2,3,4,5] 的一个子序列而 [1,5,3] 不是。

示例 1：

```
输入：nums1 = [2,1,-2,5], nums2 = [3,0,-6]
输出：18
解释：从 nums1 中得到子序列 [2,-2] ，从 nums2 中得到子序列 [3,-6] 。
它们的点积为 (2*3 + (-2)*(-6)) = 18 。 
```

示例 2：

```
输入：nums1 = [3,-2], nums2 = [2,-6,7]
输出：21
解释：从 nums1 中得到子序列 [3] ，从 nums2 中得到子序列 [7] 。
它们的点积为 (3*7) = 21 。
```
示例 3：
```
输入：nums1 = [-1,-1], nums2 = [1,1]
输出：-1
解释：从 nums1 中得到子序列 [-1] ，从 nums2 中得到子序列 [1] 。
它们的点积为 -1 。
```

提示：
- 1 <= nums1.length, nums2.length <= 500
- 1000 <= nums1[i], nums2[i] <= 100

![](3.png)

### 思路

比赛时没有时间来看这道题。

此题考点是dp，与LCS问题比较类似，可以参考[题解](https://leetcode-cn.com/problems/max-dot-product-of-two-subsequences/solution/fen-xiang-yi-xia-si-kao-guo-cheng-c-by-time-limit/)

### 代码

```c++
class Solution {
public:
    int maxDotProduct(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        
        for(int i = 1; i <= m; ++i){
            for(int j = 1; j <= n; ++j){
                dp[i][j] = nums1[i-1]*nums2[j-1];
                dp[i][j] = max(dp[i][j],nums1[i-1]*nums2[j-1] + dp[i-1][j-1]);
                if(i > 1) dp[i][j] = max(dp[i][j],dp[i-1][j]);
                if(j > 1) dp[i][j] = max(dp[i][j],dp[i][j-1]);
            }
        }
        
        return dp[m][n];
    }
};
```