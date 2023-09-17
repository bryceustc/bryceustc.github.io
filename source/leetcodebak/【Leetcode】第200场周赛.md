---
title: 【Leetcode】第200场周赛
date: 2020-08-02 18:35:32
tags: LeetCode
categories: LeetCode
mathjax: true
---
## 第200场周赛

上周学校有事没有参加周赛，这次做周赛感觉有点陌生，下周就要从快手离职了，这三个月自己学到了很多，自己也好好整理，珍惜这段经历，不断的提升自己，从现在开始要认真准备学校的事情和秋招了，冲冲冲！

### [5475. 统计好三元组](https://leetcode-cn.com/problems/count-good-triplets/)

给你一个整数数组 arr ，以及 a、b 、c三个整数。请你统计其中好三元组的数量。

如果三元组 (arr[i], arr[j], arr[k]) 满足下列全部条件，则认为它是一个好三元组 。

- 0 <= i < j < k < arr.length
- |arr[i] - arr[j]| <= a
- |arr[j] - arr[k]| <= b
- |arr[i] - arr[k]| <= c

其中 |x| 表示 x的绝对值。

返回 好三元组的数量 。
<!--more-->
示例1：
```
输入：arr = [3,0,1,1,9,7], a = 7, b = 2, c = 3
输出：4
解释：一共有 4 个好三元组：[(3,0,1), (3,0,1), (3,1,1), (0,1,1)] 。
```

示例2:
```
输入：arr = [1,1,2,2,3], a = 0, b = 0, c = 1
输出：0
解释：不存在满足所有条件的三元组。
```

提示：
- 3 <= arr.length <= 100
- 0 <= arr[i] <= 1000
- 0 <= a, b, c <= 1000
### 思路

直接暴力模拟

### 代码
```c++
class Solution {
public:
    int countGoodTriplets(vector<int>& arr, int a, int b, int c) {
        int res = 0;
        int n = arr.size();
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i+1; j < n-1; ++j) {
                for (int k = j+1; k < n; ++k) {
                    if (abs(arr[i] - arr[j]) <= a && abs(arr[j] - arr[k]) <= b && abs(arr[i] - arr[k]) <= c) {
                        res++;
                    }
                }
            }
        }
        return res;
    }
};
```

### [5476. 找出数组游戏的赢家](https://leetcode-cn.com/problems/find-the-winner-of-an-array-game/)

给你一个由 不同 整数组成的整数数组 arr 和一个整数 k 。

每回合游戏都在数组的前两个元素（即 arr[0] 和 arr[1] ）之间进行。比较 arr[0] 与 arr[1] 的大小，较大的整数将会取得这一回合的胜利并保留在位置 0 ，较小的整数移至数组的末尾。当一个整数赢得 k 个连续回合时，游戏结束，该整数就是比赛的 赢家 。

返回赢得比赛的整数。

题目数据 保证 游戏存在赢家。


示例 1：

```
输入：arr = [2,1,3,5,4,6,7], k = 2
输出：5
解释：一起看一下本场游戏每回合的情况：
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/30/q-example.png)
因此将进行 4 回合比赛，其中 5 是赢家，因为它连胜 2 回合。

示例 2：
```
输入：arr = [3,2,1], k = 10
输出：3
解释：3 将会在前 10 个回合中连续获胜。
```

示例3：
```
输入：arr = [1,9,8,2,3,7,6,4,5], k = 7
输出：9
```

示例4：
```
输入：arr = [1,11,22,33,44,55,66,77,88,99], k = 1000000000
输出：99
```

提示：
- 2 <= arr.length <= 10^5
- 1 <= arr[i] <= 10^6
- arr 所含的整数 各不相同 。
- 1 <= k <= 10^9

### 思路
此题比赛时超时，通过182/185个用例，

### 代码
```c++
/***超时代码O(n^2)***/
class Solution {
public:
    int getWinner(vector<int>& arr, int k) {
        int res = 0;
        int n = arr.size();
        int count = 0;
        // k > n 一定是最大值
        if (k >= n) {
            res = *max_element(arr.begin(), arr.end());
            return res;
        }
        // 没必要这样结束循环
        while (count < k) {
            int a = arr[0];
            int b = arr[1];
            arr[0] = max(a,b);
            arr[1] = min(a,b);
            if (arr[0] == a) {
                count++;
                res = a;
            } else {
                res = b;
                count = 1;
            }
            arr.erase(arr.begin()+1);
            arr.push_back(arr[1]);
        }
        return res;
    }
};

/*时间复杂度O(N)的代码*/
class Solution {
public:
    int getWinner(vector<int>& arr, int k) {
        int n = arr.size();
        // res:胜利者,count:获胜场次
        int count = 0;
        int res = arr[0];
        for (int i = 1; i < n && count < k; ++i ){
            if (res > arr[i]) { //arr[i]比res小*的情况
                count++;
            } else { // 如果res输掉了产生新的胜利者，count置为1
                res = arr[i];
                count = 1;
            }
        }
        // 如果在遇见arr里面最大值之前都没找到满足条件的胜利者。那么无论k多大，arr中的最大值都是胜利者。
        // 遍历一遍都没到达k,直接返回最大值
        return res;
    }
};
```

### [5477. 排布二进制网格的最少交换次数](https://leetcode-cn.com/problems/minimum-swaps-to-arrange-a-binary-grid/)

给你一个 n x n 的二进制网格 grid，每一次操作中，你可以选择网格的 相邻两行 进行交换。

一个符合要求的网格需要满足主对角线以上的格子全部都是 0 。

请你返回使网格满足要求的最少操作次数，如果无法使网格符合要求，请你返回 -1 。

主对角线指的是从 (1, 1) 到 (n, n) 的这些格子。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/fw.jpg)
```
输入：grid = [[0,0,1],[1,1,0],[1,0,0]]
输出：3
```

示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/e2.jpg)
```
输入：grid = [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]
输出：-1
解释：所有行都是一样的，交换相邻行无法使网格符合要求。
```

示例 3：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/e3.jpg)
```
输入：grid = [[1,0,0],[1,1,0],[1,1,1]]
输出：0
```

提示：
- n == grid.length
- n == grid[i].length
- 1 <= n <= 200
- grid[i][j] 要么是 0 要么是 1 。

### 思路

比赛思路先统计每行后缀0的个数，然后按照冒泡排序来贪心，但是只通过了106/129。

正经思路：

要想实现对角线以上格子全是0，那么我们只需要记录，每一行从后往前遍历，连续0的个数。

并且，（假设是n x n网格）

第一行是后缀0必须大于等于n - 1

第二行后缀0必须大于等于n - 2

……

0

我们可以从上往下贪心分配符合要求的行，因为主对角线是左上到右下的，按照题目的要求元素为 0 的要求逐渐变宽松。换句话说，一行如果能让第 i 行如何要求，一定能让第 j (j > i) 行符合要求。

**贪心的思路：**

- 从第一行开始，如果该行的后缀0满足条件，那么直接跳过进入下一行（因为需要的后缀0个数是从大到小的顺序，所以不必担心前面的会抢后面的）

- 如果该行后缀0个数不满足条件，那么就往下遍历找到最先（贪心，这是最小次数）满足条件的行，一行一行换上来，记录交换的次数（因为题目条件是只能相邻行之间交换，即使换的途中，中间某一行出现了符合的情况，若其上一行不满足后缀0个数，我们还是得移动）

- 如果找不到满足条件的后缀0，那么就返回-1.

### 代码

**比赛思路：**
```c++
class Solution {
public:
    int minSwaps(vector<vector<int>>& grid) {
        int res = 0;
        int n = grid.size();
        vector<int> dp(n,0);
        for (int i = 0; i < n; i++) {
            for (int j = n-1; j >= 0; --j) {
                if (j < n-1 && grid[i][j]==0 && grid[i][j] == grid[i][j+1]) {
                    dp[i]++;
                } else if (j == n-1 && grid[i][j] == 0) {
                    dp[i]=1;
                } else {
                    break;
                }
            }
        }
        vector<int> tmp(dp.begin(), dp.end());
        sort(tmp.begin(),tmp.end());
        for (int i = n-1; i >=0; i--) {
            // cout << tmp[i] << endl;
            if (tmp[i] < i) {
                return -1;
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n - 1 - i; j++) {
                if (dp[j] < dp[j+1]) {
                    // cout << dp[j] << endl;
                    // cout << dp[j+1] << endl;
                    
                    int k = 0;
                    int count = 0;
                    cout << "abc" << endl;
                    for (k = 0; k < n; k++) {
                        cout << dp[k] << endl;
                        if (dp[k] >= n-1-k) {
                            count++;
                        }
                    }
                    if (count==n) return res;
                    swap(dp[j],dp[j+1]);
                    res++;
                }
                
            }
        }
        return res;
    }
};
```

**正确解答**
```c++
class Solution {
public:
    int minSwaps(vector<vector<int>>& grid) {
        int count = 0;
        int n = grid.size();
        vector<int> zero(n,0);  //记录每一行后缀0个数的数组
        for (int i = 0; i < n; i++){
            for (int j = n-1; j >=0; j--) {
                if (grid[i][j] == 0) {
                    zero[i]++;
                }
                else {
                    break;
                }
            }
        }
        for (int i = 0; i < n; i++) {
            if (zero[i] >= n - 1 - i) continue;  //满足条件，该行直接跳过
            else {  //不满足条件
                int j = i;  //用新参数遍历找满足条件的后缀0
                for (; j < n; j++){
                    if (zero[j] >= n - 1 - i) {
                        break;
                    }
                }
                // 未找到符合要求的 直接返回-1
                if (j == n) return - 1;
                //找到了最先满足条件的后缀0个数
                for (; j > i; j-- ) {
                    swap(zero[j], zero[j-1]);  //每一行交换上去（类似冒泡）
                    count++;  //记录交换次数
                }
            }
        }
        return count;
    }
};
```

### [5478. 最大得分](https://leetcode-cn.com/problems/get-the-maximum-score/)

你有两个 有序 且数组内元素互不相同的数组 nums1 和 nums2 。

一条 合法路径 定义如下：

选择数组 nums1 或者 nums2 开始遍历（从下标 0 处开始）。
从左到右遍历当前数组。
如果你遇到了 nums1 和 nums2 中都存在的值，那么你可以切换路径到另一个数组对应数字处继续遍历（但在合法路径中重复数字只会被统计一次）。
得分定义为合法路径中不同数字的和。

请你返回所有可能合法路径中的最大得分。

由于答案可能很大，请你将它对 10^9 + 7 取余后返回。

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/sample_1_1893.png)

```
输入：nums1 = [2,4,5,8,10], nums2 = [4,6,8,9]
输出：30
解释：合法路径包括：
[2,4,5,8,10], [2,4,5,8,9], [2,4,6,8,9], [2,4,6,8,10],（从 nums1 开始遍历）
[4,6,8,9], [4,5,8,10], [4,5,8,9], [4,6,8,10]  （从 nums2 开始遍历）
最大得分为上图中的绿色路径 [2,4,6,8,10] 。
```

示例 2：
```
输入：nums1 = [1,3,5,7,9], nums2 = [3,5,100]
输出：109
解释：最大得分由路径 [1,3,5,100] 得到。
```
示例 3：

```
输入：nums1 = [1,2,3,4,5], nums2 = [6,7,8,9,10]
输出：40
解释：nums1 和 nums2 之间无相同数字。
最大得分由路径 [6,7,8,9,10] 得到。
```

示例4：
```
输入：nums1 = [1,4,5,8,9,11,19], nums2 = [2,3,4,11,12]
输出：61
```

提示：
- 1 <= nums1.length <= 10^5
- 1 <= nums2.length <= 10^5
- 1 <= nums1[i], nums2[i] <= 10^7
- nums1 和 nums2 都是严格递增的数组。

### 思路

相交点可以将2个数组都分成(K + 1)段，统计每段的和，并取较大值计入结果，可以用双指针快速实现。

![](1.jpg)

本质类似合并两个有序数组，类似归并排序
- 每次推进值较小的指针并记录当前段的和。
- 当双指针值相同时对当前遍历过的值做一次“总结”，取两段当前和的较大值，加入总和中。
- 完毕后重置当前和为0，并同时推进双指针，重复之前的分段+取较大和的过程。
需要考虑边界的条件：最后一段可能不会以相同值结束，所以需要在遍历结束后单独将最后一段的结果加进总和里。

### 代码

```c++
class Solution {
public:
    int maxSum(vector<int>& nums1, vector<int>& nums2) {
        long long res  = 0;
        const int MOD = 1e9+7;
        int n = nums1.size();
        int m = nums2.size();
        int i = 0, j = 0;
        long sumA = 0, sumB = 0;
        while (i < n && j < m) {
            if (nums1[i] == nums2[j]){
                res += max(sumA, sumB) + nums1[i];
                sumA = 0;
                sumB = 0;
                i++;
                j++;
            } else if (nums1[i] < nums2[j]) {
                sumA += nums1[i];
                i++;
            } else {
                sumB += nums2[j];
                j++;
            }
        }

        while (i < n) {
            sumA += nums1[i];
            i++;
        }

        while (j < m){
            sumB += nums2[j];
            j++;
        }
        res += max(sumA, sumB);
        return res % MOD;
    }
};
```