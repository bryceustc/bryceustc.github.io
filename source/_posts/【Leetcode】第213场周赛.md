---
title: 【Leetcode】第213场周赛
date: 2020-11-01 14:54:23
tags: LeetCode
categories: LeetCode
mathjax: true
---
2020.11.1, 第213场周赛, ac 3/4 , 第四题dfs超时，排名655，新的一个月了，继续加油吧小伙子，做一个终身学习者，想自己开一个公众号，记录自己的学习笔记。
![](1.png)
### [5554. 能否连接形成数组](https://leetcode-cn.com/problems/check-array-formation-through-concatenation/)

给你一个整数数组 arr ，数组中的每个整数 互不相同 。另有一个由整数数组构成的数组 pieces，其中的整数也 互不相同 。请你以 任意顺序 连接 pieces 中的数组以形成 arr 。但是，不允许 对每个数组 pieces[i] 中的整数重新排序。

如果可以连接 pieces 中的数组形成 arr ，返回 true ；否则，返回 false 。

<!--more-->
示例1：
```
输入：arr = [85], pieces = [[85]]
输出：true
```

示例2:
```
输入：arr = [15,88], pieces = [[88],[15]]
输出：true
解释：依次连接 [15] 和 [88]
```

示例3：
```
输入：arr = [49,18,16], pieces = [[16,18,49]]
输出：false
解释：即便数字相符，也不能重新排列 pieces[0]
```

提示：
- 1 <= pieces.length <= arr.length <= 100
- sum(pieces[i].length) == arr.length
- 1 <= pieces[i].length <= arr.length
- 1 <= arr[i], pieces[i][j] <= 100
- arr 中的整数 互不相同
- pieces 中的整数 互不相同（也就是说，如果将 pieces 扁平化成一维数组，数组中的所有整数互不相同）

### 思路

理解题意之后直接暴力循环模拟即可。注意相同的字符，不同的持续时间，不是叠加的关系。

### 代码
```c++
class Solution {
public:
    bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
        for (int i = 0; i < arr.size(); i++) {
            int a = arr[i];
            int flag = 0;
            for (int j = 0; j < pieces.size(); j++) {
                if (!flag) {
                    for (int k = 0; k < pieces[j].size(); k++) {
                        if (a == pieces[j][k]) {
                            while (k < pieces[j].size()-1) {
                                int a = arr[++i];
                                int b = pieces[j][++k];
                                if (a != b) return false;
                            }
                            flag = 1;
                            break;
                        }
                    }
                }
            }
            if (flag == 0) return false;
        }
        return true;
    }
};
```

### [5555. 统计字典序元音字符串的数目](https://leetcode-cn.com/problems/count-sorted-vowel-strings/)

给你一个整数 n，请返回长度为 n 、仅由元音 (a, e, i, o, u) 组成且按 字典序排列 的字符串数量。

字符串 s 按 字典序排列 需要满足：对于所有有效的 i，s[i] 在字母表中的位置总是与 s[i+1] 相同或在 s[i+1] 之前。

示例 1：
```
输入：n = 1
输出：5
解释：仅由元音组成的 5 个字典序字符串为 ["a","e","i","o","u"]
```

示例 2：
```
输入：n = 2
输出：15
解释：仅由元音组成的 15 个字典序字符串为
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"]
注意，"ea" 不是符合题意的字符串，因为 'e' 在字母表中的位置比 'a' 靠后
```
示例 3：
```
输入：n = 33
输出：66045
```

提示：
- 1 <= n <= 50 

### 思路

dfs

dp

组合计数
### 代码
```c++
class Solution {
public:
    int countVowelStrings(int n) {
        res = 0;
        string s = "aeiou";
        string path;
        dfs(s,n,0, path);
        return res;
    }
    void dfs(string s, int n, int level, string &path) {
        if (level == n) {
            res++;
            return;
        }
        for (int i = 0; i < s.size(); i++) {
            if (!path.empty() && (path.back() -'a') > (s[i] - 'a')) continue;
            path.push_back(s[i]);
            dfs(s,n,level+1, path);
            path.pop_back();
        }
    }
private:
    int res;
};
```

dp法

```c++
class Solution {
    public int countVowelStrings(int n) {
        // 定义dp[n][5]数组，其中dp[i][0-4]表示长度为i的以a-u结尾的字符串的个数
        int dp[][] = new int[n+1][5];

        //初始化n=1的情况
        for (int i=0;i<5;i++){
            dp[1][i]=1;
        }

        for (int i=2;i<=n;i++){
            //长度i的以u结尾的字符串可以由任意一个长度i-1的字符串结尾加个u得到
            dp[i][4]=dp[i-1][0]+dp[i-1][1]+dp[i-1][2]+dp[i-1][3]+dp[i-1][4];
            dp[i][3]=dp[i-1][0]+dp[i-1][1]+dp[i-1][2]+dp[i-1][3];
            dp[i][2]=dp[i-1][0]+dp[i-1][1]+dp[i-1][2];
            dp[i][1]=dp[i-1][0]+dp[i-1][1];
            //长度i的以a结尾的字符串只能由长度i-1的以a结尾的字符串结尾加个a得到
            dp[i][0]=dp[i-1][0];
        }

        //最终答案求个和就行啦
        return dp[n][0]+dp[n][1]+dp[n][2]+dp[n][3]+dp[n][4];
    }
}
```

组合计数法，参考[这里](https://leetcode-cn.com/problems/count-sorted-vowel-strings/solution/tong-ji-zi-dian-xu-yuan-yin-zi-fu-chuan-de-shu-mu-/)
```c++
class Solution {
public:
    int countVowelStrings(int n) {
        return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
    }
};
```

### [5556. 可以到达的最远建筑](https://leetcode-cn.com/problems/furthest-building-you-can-reach/)

给你一个整数数组 heights ，表示建筑物的高度。另有一些砖块 bricks 和梯子 ladders 。

你从建筑物 0 开始旅程，不断向后面的建筑物移动，期间可能会用到砖块或梯子。

当从建筑物 i 移动到建筑物 i+1（下标 从 0 开始 ）时：

如果当前建筑物的高度 大于或等于 下一建筑物的高度，则不需要梯子或砖块
如果当前建筑的高度 小于 下一个建筑的高度，您可以使用 一架梯子 或 (h[i+1] - h[i]) 个砖块
如果以最佳方式使用给定的梯子和砖块，返回你可以到达的最远建筑物的下标（下标 从 0 开始 ）。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/31/q4.gif)
```
输入：heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
输出：4
解释：从建筑物 0 出发，你可以按此方案完成旅程：
- 不使用砖块或梯子到达建筑物 1 ，因为 4 >= 2
- 使用 5 个砖块到达建筑物 2 。你必须使用砖块或梯子，因为 2 < 7
- 不使用砖块或梯子到达建筑物 3 ，因为 7 >= 6
- 使用唯一的梯子到达建筑物 4 。你必须使用砖块或梯子，因为 6 < 9
无法越过建筑物 4 ，因为没有更多砖块或梯子。
```

示例 2：
```
输入：heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
输出：7
```

示例 3:
```
输入：heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
输出：7
```

提示：
- 1 <= heights.length <= 105
- 1 <= heights[i] <= 106
- 0 <= bricks <= 109
- 0 <= ladders <= heights.length

### 思路

优先队列+贪心

- 显然，在一段旅程中，需要将梯子用在差距较大的楼层之间。
- 此时，这个问题就变成了求动态前 k 大，梯子的个数就是 k。前 k 大的高度差使用梯子，剩余的高度差使用砖块。
- 对于剩余的 Δh，我们需要使用砖块，因此需要对它们进行累加，如果某一时刻这个累加值超过了砖块的数目 bb，那么我们就再也无法移动了。


时间复杂度：O(n \log l)O(nlogl)，其中 n 是建筑物的数量，l 是梯子的数量。

空间复杂度：O(l)O(l)O(l)。
### 代码

```c++
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        int res = 0;
        int n = heights.size();
        priority_queue<int, vector<int>, greater<int>> q; // 小顶堆
        int sumH = 0;
        for (int i = 1; i < n; i++) {
            int deltaH = heights[i] - heights[i-1];
            if (deltaH > 0) {
                q.push(deltaH);
                if (q.size() > ladders) {
                    sumH += q.top();
                    q.pop();
                }
                if (sumH > bricks) {
                    return i - 1;
                }
            }
        }
        return n - 1;
    }
};
```

### [5600. 第 K 条最小指令](https://leetcode-cn.com/problems/kth-smallest-instructions/)

Bob 站在单元格 (0, 0) ，想要前往目的地 destination ：(row, column) 。他只能向 右 或向 下 走。你可以为 Bob 提供导航 指令 来帮助他到达目的地 destination 。

指令 用字符串表示，其中每个字符：

'H' ，意味着水平向右移动
'V' ，意味着竖直向下移动
能够为 Bob 导航到目的地 destination 的指令可以有多种，例如，如果目的地 destination 是 (2, 3)，"HHHVV" 和 "HVHVH" 都是有效 指令 。

然而，Bob 很挑剔。因为他的幸运数字是 k，他想要遵循 按字典序排列后的第 k 条最小指令 的导航前往目的地 destination 。k  的编号 从 1 开始 。

给你一个整数数组 destination 和一个整数 k ，请你返回可以为 Bob 提供前往目的地 destination 导航的 按字典序排列后的第 k 条最小指令 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/01/ex1.png)
```
输入：destination = [2,3], k = 1
输出："HHHVV"
解释：能前往 (2, 3) 的所有导航指令 按字典序排列后 如下所示：
["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVHH", "VVHHH"].
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/01/ex2.png)
```
输入：destination = [2,3], k = 2
输出："HHVHV"
```

示例3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/01/ex3.png)
```
输入：destination = [2,3], k = 3
输出："HHVVH"
```

提示：
- destination.length == 2
- 1 <= row, column <= 15
- 1 <= k <= nCr(row + column, row)，其中 nCr(a, b) 表示组合数，即- 从 a 个物品中选 b 个物品的不同方案数。

### 思路

1. 首先想到的是暴力深搜，然后暴力超时


2. 优先确定高位 + 组合计数

### 代码
```c++
class Solution {
public:
    string kthSmallestPath(vector<int>& destination, int k) {
        int v = destination[0];
        int h = destination[1];
        string s = "HV";
        int n = v + h;
        dfs (s, n, v, h, 0);
        sort(path.begin(), path.end());
        res = path[k-1];
        return res;
    }
    void dfs(string s, int n, int v, int h, int level) {
        if (level == n) {
            // count 统计字符串中，某个字符出现的次数
            int x = count(ans.begin(), ans.end(), 'H');
            int y = count(ans.begin(), ans.end(), 'V');
            if (x == h && y == v) path.push_back(ans);
            return;
        }
        for (int i = 0; i < s.size(); i++) {
            ans.push_back(s[i]);
            dfs (s, n, v, h, level+1);
            ans.pop_back();
        }
    }
private:
    string res;
    vector<string> path;
    string ans;
};
```


组合计数
```c++
class Solution {
public:
    string kthSmallestPath(vector<int>& destination, int k) {
        string res;
        int m = destination[0];
        int n = destination[1];
        // 预处理组合数，利用递推公式来求
        const int MAXN = 35;
        long long C[MAXN][MAXN];
        C[0][0] = 1;
        for (int i = 1; i < MAXN; ++i) {
            C[i][0] = C[i][i] = 1;
            for (int j = 1; j < i; ++j) {
                C[i][j] = C[i - 1][j - 1] + C[i - 1][j];
            }
        }
        //根据高位字母进行区间划分
        for (int i = 1, lim = n + m; i <= lim; i++) {
            if (n >= 1 && C[n - 1 + m][m] >= k) {
                res+= "H";
                n--;
            } else {
                res += "V";
                k -= C[n - 1 + m][m];
                m--;
            }
        }
        return res;
    }

    // 计算组合数的公式，乘法再除法，产生了溢出
    long long factorial(long long n)
    {
        long long fc=1;
        for(long long i=1;i<=n;++i) fc *= i;
        return fc;
    }
    long long combo(long long n, long long m) {
        long long com=factorial(n)/(factorial(m)*factorial(n-m));
        return com;
    }
    
};
```