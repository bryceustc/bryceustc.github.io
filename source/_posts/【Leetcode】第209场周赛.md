---
title: 【Leetcode】第209场周赛
date: 2020-10-04 12:32:02
vtags: LeetCode
categories: LeetCode
mathjax: true
---
### [5531. 特殊数组的特征值](https://leetcode-cn.com/problems/special-array-with-x-elements-greater-than-or-equal-x/)
给你一个非负整数数组 nums 。如果存在一个数 x ，使得 nums 中恰好有 x 个元素 大于或者等于 x ，那么就称 nums 是一个 特殊数组 ，而 x 是该数组的 特征值 。

注意： x 不必 是 nums 的中的元素。

如果数组 nums 是一个 特殊数组 ，请返回它的特征值 x 。否则，返回 -1 。可以证明的是，如果 nums 是特殊数组，那么其特征值 x 是 唯一的 。
<!--more-->
示例1：
```
输入：nums = [3,5]
输出：2
解释：有 2 个元素（3 和 5）大于或等于 2 。
```

示例2:
```
输入：nums = [0,0]
输出：-1
解释：没有满足题目要求的特殊数组，故而也不存在特征值 x 。
如果 x = 0，应该有 0 个元素 >= x，但实际有 2 个。
如果 x = 1，应该有 1 个元素 >= x，但实际有 0 个。
如果 x = 2，应该有 2 个元素 >= x，但实际有 0 个。
x 不能取更大的值，因为 nums 中只有两个元素。
```

示例3：
```
输入：nums = [0,4,3,0,4]
输出：3
解释：有 3 个元素大于或等于 3 。
```

提示：
- 1 <= nums.length <= 100
- 0 <= nums[i] <= 1000
### 思路

直接暴力循环模拟即可。

### 代码
```c++
class Solution {
public:
    int specialArray(vector<int>& nums) {
        int n = nums.size();
        int res = -1;
        for (int x = 0; x <= n; x++) {
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                if (nums[j] >= i) {
                    cnt++;
                }
            }
            if (cnt == x) return x;
        }
        return res;
    }
};
```

### [5532. 奇偶树](https://leetcode-cn.com/problems/even-odd-tree/)

如果一棵二叉树满足下述几个条件，则可以称为 奇偶树 ：

二叉树根节点所在层下标为 0 ，根的子节点所在层下标为 1 ，根的孙节点所在层下标为 2 ，依此类推。

偶数下标 层上的所有节点的值都是 奇 整数，从左到右按顺序 严格递增

奇数下标 层上的所有节点的值都是 偶 整数，从左到右按顺序 严格递减

给你二叉树的根节点，如果二叉树为 奇偶树 ，则返回 true ，否则返回 false 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/sample_1_1966.png)
```
输入：root = [1,10,4,3,null,7,9,12,8,6,null,null,2]
输出：true
解释：每一层的节点值分别是：
0 层：[1]
1 层：[10,4]
2 层：[3,7,9]
3 层：[12,8,6,2]
由于 0 层和 2 层上的节点值都是奇数且严格递增，而 1 层和 3 层上的节点值都是偶数且严格递减，因此这是一棵奇偶树。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/sample_2_1966.png)
```
输入：root = [5,4,2,3,3,7]
输出：false
解释：每一层的节点值分别是：
0 层：[5]
1 层：[4,2]
2 层：[3,3,7]
2 层上的节点值不满足严格递增的条件，所以这不是一棵奇偶树。
```

示例3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/sample_1_333_1966.png)
```
输入：root = [5,9,1,3,5,7]
输出：false
解释：1 层上的节点值应为偶数。
```

示例4：
```
输入：root = [1]
输出：true
```

示例5：
```
输入：root = [11,8,6,1,3,9,11,30,20,18,16,12,10,4,2,17]
输出：true
```

提示：
- 树中节点数在范围 [1, 10^5] 内
- 1 <= Node.val <= 10^6

### 思路
层序遍历

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
    bool isEvenOddTree(TreeNode* root) {
        if (root == NULL) return true;
        bool flag = false;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            // 奇数层
            if (flag) {
                vector<int> A;
                int n = q.size();
                while (n--) {
                    auto t = q.front();
                    A.push_back(t->val);
                    q.pop();
                    if (t->left) q.push(t->left);
                    if (t->right) q.push(t->right);
                }
                for (int i = 0; i < A.size();i++) {
                    if (A[i] % 2 != 0) return false;
                    if (i > 0 && A[i-1] <= A[i]) return false;
                }
            } else {
                // 偶数层
                int n = q.size();
                vector<int> A;
                while (n--) {
                    auto t = q.front();
                    q.pop();
                    A.push_back(t->val);
                    if (t->left) q.push(t->left);
                    if (t->right) q.push(t->right);
                }
                for (int i = 0; i < A.size();i++) {
                    if (A[i] % 2 == 0) return false;
                    if (i > 0 && A[i-1] >= A[i]) return false;
                }
            }
            flag = !flag;
        }
        return true;
    }
};
```

### [5534. 可见点的最大数目](https://leetcode-cn.com/contest/weekly-contest-209/problems/maximum-number-of-visible-points/)

给你一个点数组 points 和一个表示角度的整数 angle ，你的位置是 location ，其中 location = [posx, posy] 且 points[i] = [xi, yi] 都表示 X-Y 平面上的整数坐标。

最开始，你面向东方进行观测。你 不能 进行移动改变位置，但可以通过 自转 调整观测角度。换句话说，posx 和 posy 不能改变。你的视野范围的角度用 angle 表示， 这决定了你观测任意方向时可以多宽。设 d 为逆时针旋转的度数，那么你的视野就是角度范围 [d - angle/2, d + angle/2] 所指示的那片区域。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/16/q3v1.jpg)
```
输入：position = [1,2,3,4,7], m = 3
输出：3
解释：将 3 个球分别放入位于 1，4 和 7 的三个篮子，两球间的磁力分别为 [3, 3, 6]。最小磁力为 3 。我们没办法让最小磁力大于 3 。
```

示例 2：
```
输入：position = [5,4,3,2,1,1000000000], m = 2
输出：999999999
解释：我们使用位于 1 和 1000000000 的篮子时最小磁力最大。
```

提示：
- n == position.length
- 2 <= n <= 10^5
- 1 <= position[i] <= 10^9
- 所有 position 中的整数 互不相同 。
- 2 <= m <= position.length

### 思路

碰到最大化最小值，或者最小化最大值都是利用二分来做。一定要熟练掌握y总给的二分模板！！

- 首先找到二分查找的左右边界
- 然后每次用O(n)的复杂判断是否满足题目要求
- 最终确定二分中的最大或最小目标值。

### 代码

```c++
class Solution {
public:
    int maxDistance(vector<int>&a, int m) {
        int n = a.size();
        sort(a.begin(), a.end());
        int l = 0;
        int r = a[n-1];
        while (l < r) {
            int mid = l + (r -  l + 1) / 2;
            if (check(mid, a, m)) {
                l = mid; //mid是符合的，mid也可以更大，最优解在右半区间
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    bool check(int mid, vector<int> &a, int m) {
        int last = a[0];
        int n = a.size();
        int cnt = 1;  //符合相邻两个点之间的距离>=mid的个数
        for (int i = 1; i < n; i++) {
            if (a[i] - last >= mid) {
                last = a[i];
                cnt++;
            }
        }
        return cnt >= m;
    }
};
```

### [5490. 吃掉 N 个橘子的最少天数](https://leetcode-cn.com/problems/minimum-cost-to-cut-a-stick/)

厨房里总共有 n 个橘子，你决定每一天选择如下方式之一吃这些橘子：

吃掉一个橘子。
如果剩余橘子数 n 能被 2 整除，那么你可以吃掉 n/2 个橘子。
如果剩余橘子数 n 能被 3 整除，那么你可以吃掉 2*(n/3) 个橘子。
每天你只能从以上 3 种方案中选择一种方案。

请你返回吃掉所有 n 个橘子的最少天数。

示例 1：

```
输入：n = 10
输出：4
解释：你总共有 10 个橘子。
第 1 天：吃 1 个橘子，剩余橘子数 10 - 1 = 9。
第 2 天：吃 6 个橘子，剩余橘子数 9 - 2*(9/3) = 9 - 6 = 3。（9 可以被 3 整除）
第 3 天：吃 2 个橘子，剩余橘子数 3 - 2*(3/3) = 3 - 2 = 1。
第 4 天：吃掉最后 1 个橘子，剩余橘子数 1 - 1 = 0。
你需要至少 4 天吃掉 10 个橘子。
```

示例 2：
```
输入：n = 6
输出：3
解释：你总共有 6 个橘子。
第 1 天：吃 3 个橘子，剩余橘子数 6 - 6/2 = 6 - 3 = 3。（6 可以被 2 整除）
第 2 天：吃 2 个橘子，剩余橘子数 3 - 2*(3/3) = 3 - 2 = 1。（3 可以被 3 整除）
第 3 天：吃掉剩余 1 个橘子，剩余橘子数 1 - 1 = 0。
你至少需要 3 天吃掉 6 个橘子。
```

示例3：
```
输入：n = 1
输出：1
```

示例4：
```
输入：n = 56
输出：6
```

提示：
- 1 <= n <= 2*10^9

### 思路

区间dp问题。（奈何我之前不会，多总结多学习）

### 代码

比赛超时代码（dp)
```c++
class Solution {
public:
    int minDays(int n) {
        vector<int> dp(n+1, n+1);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            if ((i &1) ==0 && i%3 == 0) {
                dp[i] = min(min(dp[i/2] + 1, dp[i/3]+1), dp[i-1] +1);
            } else if ((i &1) ==0 && i%3 != 0) {
                dp[i] = min(dp[i/2] + 1, dp[i-1]+1);
            } else if ((i &1) !=0 && i%3 == 0) {
                dp[i] = min(dp[i/3] + 1, dp[i-1]+1);
            } else {
                dp[i] = dp[i-1] + 1;
            }
        }
        return dp[n];
    }
};
```

优化的记忆化递归搜索
```c++
class Solution {
public:
    unordered_map<int, int> dp; //记录已经计算完成的n的值
    int minDays(int n) {
        return dfs(n);
    }
    int dfs (int n) {
        if (n == 1) return 1;
        if (n == 2 || n == 3) return 2;

        if (dp.count(n)) return dp[n];
        //最经典的地方， 对 n 整除 2  dfs(n / 2) + n % 2 + 1 ， 对 n 整除 3  dfs(n / 3) + n % 3 + 1
        int res = min(dfs(n/2) + 1 + n % 2, dfs(n / 3) + 1 + n % 3);
        dp[n] = res;
        return dp[n];
    }
};
```

求最短路想到bfs
```c++
class Solution {
public:
    unordered_map<int, int> dp;
    queue<int> q;

    void extend(int dist, int x) {
        // 已经计算过了，直接返回
        if (dp.count(x)) return;
        dp[x] = dist + 1;
        q.push(x);
    }

    int minDays(int n) {
        if (n==0 || n == 1) return n;
        q.push(n);
        while (!q.empty()) {
            int t = q.front();
            q.pop();
            // t == 0表示吃完了 直接返回
            if (t == 0) return dp[t];
            extend(dp[t], t - 1);
            if (t % 3 == 0) extend(dp[t], t/3);
            if (t % 2 == 0) extend(dp[t], t/2);
        }
        return -1;
    }
};
```