---
title: 【Leetcode】第196场周赛
date: 2020-07-05 16:27:47
tags: LeetCode
categories: LeetCode
---
## 第196场周赛

本周题目AC 2/4，本周题目是与字节研发提前批合作，依旧是只做出来2道题，第三题的思路有点问题但没发现，还是得再努力啊，上周说了，从这周开始贴排名，希望能不断激励自己。PS：最近真的挺累的，实习工作的事情，学校的事情，但还是要挤出时间来准备秋招啊，多学点操作系统计算机网络的相关知识，基础本来就比较薄弱还不多学一点！！总是给自己激励加油，口号喊得很多，同时也要付出行动啊，现在的执行力以及自制力真的挺差的，希望我能更专注一点，更踏实一点，不要想着一口吃成一个大胖子，慢慢来吧，加油！小伙子！先定个小目标，以后周赛完成稳定完成3道题。
![](1.jpg)

### [5452. 判断能否形成等差数列](https://leetcode-cn.com/problems/can-make-arithmetic-progression-from-sequence/)

给你一个数字数组 arr 。

如果一个数列中，任意相邻两项的差总等于同一个常数，那么这个数列就称为 等差数列 。

如果可以重新排列数组形成等差数列，请返回 true ；否则，返回 false 。
<!--more-->
示例1：
```
输入：arr = [3,5,1]
输出：true
解释：对数组重新排序得到 [1,3,5] 或者 [5,3,1] ，任意相邻两项的差分别为 2 或 -2 ，可以形成等差数列。
```

示例2:
```
输入：arr = [1,2,4]
输出：false
解释：无法通过重新排序得到等差数列。
```

提示：
- 2 <= arr.length <= 1000
- 10^6 <= arr[i] <= 10^6
### 思路

直接模拟

### 代码
```c++
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int a = arr[1] - arr[0];
        for (int i = 1; i < arr.size(); ++i) {
            if ((arr[i]-arr[i-1]) != a) return false;
        }
        return true;
    }
};
```

### [5453. 所有蚂蚁掉下来前的最后一刻](https://leetcode-cn.com/problems/last-moment-before-all-ants-fall-out-of-a-plank/)

有一块木板，长度为 n 个 单位 。一些蚂蚁在木板上移动，每只蚂蚁都以 每秒一个单位 的速度移动。其中，一部分蚂蚁向 左 移动，其他蚂蚁向 右 移动。

当两只向 不同 方向移动的蚂蚁在某个点相遇时，它们会同时改变移动方向并继续移动。假设更改方向不会花费任何额外时间。

而当蚂蚁在某一时刻 t 到达木板的一端时，它立即从木板上掉下来。

给你一个整数 n 和两个整数数组 left 以及 right 。两个数组分别标识向左或者向右移动的蚂蚁在 t = 0 时的位置。请你返回最后一只蚂蚁从木板上掉下来的时刻。


示例 1：

![](https://assets.leetcode.com/uploads/2020/06/17/ants.jpg)
```
输入：n = 4, left = [4,3], right = [0,1]
输出：4
解释：如上图所示：
-下标 0 处的蚂蚁命名为 A 并向右移动。
-下标 1 处的蚂蚁命名为 B 并向右移动。
-下标 3 处的蚂蚁命名为 C 并向左移动。
-下标 4 处的蚂蚁命名为 D 并向左移动。
请注意，蚂蚁在木板上的最后时刻是 t = 4 秒，之后蚂蚁立即从木板上掉下来。（也就是说在 t = 4.0000000001 时，木板上没有蚂蚁）。
```

示例 2：
![](https://assets.leetcode.com/uploads/2020/06/17/ants2.jpg)
```
输入：n = 7, left = [], right = [0,1,2,3,4,5,6,7]
输出：7
解释：所有蚂蚁都向右移动，下标为 0 的蚂蚁需要 7 秒才能从木板上掉落。
```
示例3：
![](https://assets.leetcode.com/uploads/2020/06/17/ants3.jpg)
```
输入：n = 7, left = [0,1,2,3,4,5,6,7], right = []
输出：7
解释：所有蚂蚁都向左移动，下标为 7 的蚂蚁需要 7 秒才能从木板上掉落。
```
示例4：
```
输入：n = 9, left = [5], right = [4]
输出：5
解释：t = 1 秒时，两只蚂蚁将回到初始位置，但移动方向与之前相反。
```
示例5：
```
输入：n = 6, left = [6], right = [0]
输出：6。
```

提示：
- 1 <= n <= 10^4
- 0 <= left.length <= n + 1
- 0 <= left[i] <= n
- 0 <= right.length <= n + 1
- 0 <= right[i] <= n
- 1 <= left.length + right.length <= n + 1
- left 和 right 中的所有值都是唯一的，并且每个值 **只能出现在二者之一**中。

### 思路
这个题有点像智力题，最最重要的一点就是蚂蚁碰头后掉头，可以当作蚂蚁可以直接”穿过“对方，即蚂蚁碰头对蚂蚁运动没有影响。然后可以转换为每一只蚂蚁从初始位置直接到离开木杆的场景。

此题与牛客网上的一道[问答题](https://www.nowcoder.com/questionTerminal/f791f68714474658a5acb5af74de18ca?toCommentId=140188)很类似，关键点就是碰撞不会影响，就当做没有发生碰撞，继续向前走。

### 代码
```c++
class Solution {
public:
    int getLastMoment(int n, vector<int>& left, vector<int>& right) {
        int l = left.size();
        int r = right.size();
        int  t = 0;
        for (int i = 0; i < l; ++i) {
            t = max(t, left[i] - 0);
        }
        for (int j = 0; j < r; ++j) {
            t = max(t, n - right[j]);
        }
        return t;
    }
};
```

### [5454. 统计全 1 子矩形](https://leetcode-cn.com/problems/count-submatrices-with-all-ones/)

给你一个只包含 0 和 1 的 rows * columns 矩阵 mat ，请你返回有多少个 子矩形 的元素全部都是 1 。

示例 1：
```
输入：mat = [[1,0,1],
            [1,1,0],
            [1,1,0]]
输出：13
解释：
有 6 个 1x1 的矩形。
有 2 个 1x2 的矩形。
有 3 个 2x1 的矩形。
有 1 个 2x2 的矩形。
有 1 个 3x1 的矩形。
矩形数目总共 = 6 + 2 + 3 + 1 + 1 = 13 。
```

示例 2：
```
输入：mat = [[0,1,1,0],
            [0,1,1,1],
            [1,1,1,0]]
输出：24
解释：
有 8 个 1x1 的子矩形。
有 5 个 1x2 的子矩形。
有 2 个 1x3 的子矩形。
有 4 个 2x1 的子矩形。
有 2 个 2x2 的子矩形。
有 2 个 3x1 的子矩形。
有 1 个 3x2 的子矩形。
矩形数目总共 = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24 。
```
示例 3：
```
输入：mat = [[1,1,1,1,1,1]]
输出：21
```

示例 4：
```c++
输入：mat = [[1,0,1],[0,1,0],[1,0,1]]
输出：5
```

提示：
- 1 <= rows <= 150
- 1 <= columns <= 150
- 0 <= mat[i][j] <= 1

### 思路

这道题可以看做是最大矩形的简化版。

思路是枚举每个位置作为矩形的右下角，然后看有多少个矩形。然后再相加就好

矩阵里每个点(i,j)统计他这行左边到他这个位置最多有几个连续的1，存为dp[i][j]。

然后对于每个点(i,j)，我们固定子矩形的右下角为(i,j)，利用dp从该行i向上找最小的数作为矩阵的宽w，。每次将宽为w的矩形个数依次加到res。


时间复杂度O(n^2m)，空间复杂度O(nm)。
### 代码
```c++
class Solution {
public:
    int numSubmat(vector<vector<int>>& mat) {
        int m = mat.size();
        int n = mat[0].size();
        vector< vector<int> > dp (m, vector<int> (n ,0));
        // dp[i][j] 表示保存以坐标[i][j]结尾的连续 1 的个
        // 类似每一行的前缀和
        for (int i = 0; i < m; ++i) dp[i][0] = mat[i][0];
        for (int i = 0; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (mat[i][j] == 1) {
                    dp[i][j] = dp[i][j-1] + 1;
                }
            }
        }
        // 
        int res = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int w = INT_MAX;
                for (int h = i; h >= 0; --h) {
                    // 找最小的数作为宽
                    w = min(w, dp[h][j]);
                    // 宽为w的有多少个就不断相加
                    /*
                        0 0 1 1  w=2, 宽为1，2有两组，所以加w就是表示以ij为右下角的矩形个数了
                        1 1 1 1  w=4
                    */
                    res += w;
                }
            }
        }
        return res;
    }
};
```

### [5455. 最多 K 次交换相邻数位后得到的最小整数](https://leetcode-cn.com/problems/minimum-possible-integer-after-at-most-k-adjacent-swaps-on-digits/)

给你一个字符串 num 和一个整数 k 。其中，num 表示一个很大的整数，字符串中的每个字符依次对应整数上的各个 数位 。

你可以交换这个整数相邻数位的数字 最多 k 次。

请你返回你能得到的最小整数，并以字符串形式返回。

示例 ：
![](https://assets.leetcode.com/uploads/2020/06/17/q4_1.jpg)
```
输入：num = "4321", k = 4
输出："1342"
解释：4321 通过 4 次交换相邻数位得到最小整数的步骤如上图所示。
```

示例2：
```
输入：num = "100", k = 1
输出："010"
解释：输出可以包含前导 0 ，但输入保证不会有前导 0 。
```

示例3：
```
输入：num = "36789", k = 1000
输出："36789"
解释：不需要做任何交换。
```

示例4：
```
输入：num = "22", k = 22
输出："22"
```

示例5：
```
输入：num = "9438957234785635408", k = 23
输出："0345989723478563548"
```

提示：
- 1 <= num.length <= 30000
- num 只包含 数字 且不含有 前导 0 。
- 1 <= k <= 10^9

### 思路

此题是用dp来求，倍增，树状倍增。
- 定义状态：dp[node][j] 存储的是 node 节点距离为 2^j 的祖先是谁。

根据定义，dp[node][0] 就是 parent[node]，即 node 的距离为 1 的祖先是 parent[node]。
- 状态转移方程：
dp[node][j] = dp[dp[node][j - 1]][j - 1]

意思是：要想找到 node 的距离 2^j 的祖先，先找到 node 的距离 2^(j - 1) 的祖先，然后，再找这个祖先的距离 2^(j - 1) 的祖先。两步得到 node 的距离为 2^j 的祖先。

这样做，状态总数是 O(nlogn)，可以使用 O(nlogn) 的时间做预处理。

之后，根据预处理的结果，可以在 O(logn) 的时间里完成每次查询：对于每一个查询 k，把 k 拆解成二进制表示，然后根据二进制表示中 1 的位置，累计向上查询。

可以参考[题解](https://leetcode-cn.com/problems/kth-ancestor-of-a-tree-node/solution/li-kou-zai-zhu-jian-ba-acm-mo-ban-ti-ban-shang-lai/)

### 代码

```c++
class TreeAncestor {
public:
    TreeAncestor(int n, vector<int>& parent) {
        // 预处理
        for (int i=0; i<n;i++){
            dp[i][0] = parent[i];
        }
        for (int j=1;j<=20;j++){
            for (int i=0;i<n;i++){
                int x = dp[i][j-1];
                if (x==-1) {
                    dp[i][j] = -1;
                }
                else {
                    dp[i][j] = dp[x][j-1];
                }
            }
        }
    }
    
    int getKthAncestor(int node, int k) {
        if (k==0 || node== -1) return node;
        // 然后二进制去拆解
        int res = node;
        // 7 = 4+ 2+ 1 ;
        int bit = 20;// 20位可以满足数据要求了
        while(k && res!=-1) {
            if (res==-1) return -1;
            if (k>=(1<<bit)){
                res = dp[res][bit];
                k-=(1<<bit);
            }
            bit--;
        }
        return res;
    }
    // 减少循环次数的一种写法 比如7：下边只循环3次

    // int getKthAncestor(int node, int k) {
    //     if (k==0 || node==-1) return node;
    //     int res = node;
    //     while(k) {
    //         int pos = ffs(k) -1;  // C++ 语言中 ffs(k) 求解出 k 的最右侧第一个 1 的位置
    //         if (dp[res][pos]==-1)return -1;
    //         if (k>=(1<<pos)) {
    //             res = dp[res][pos];
    //             k-=(1<<pos);
    //         }
    //     }
    //     return res;
    // }
private:

    // vector做成员变量初始化方式要注意一下

    // dp[i][j] 表示i位置 2^j 的祖宗节点
    vector<vector<int>> dp = vector<vector<int>>(5e4+10,vector<int>(22,-1)); // 5e4+10是节点数，比数据要求多10个，22是2^22大于数据了，其实设成18也可以
};

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor* obj = new TreeAncestor(n, parent);
 * int param_1 = obj->getKthAncestor(node,k);
 */
```