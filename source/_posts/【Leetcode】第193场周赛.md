---
title: 【Leetcode】第193场周赛
date: 2020-06-14 15:15:11
tags: LeetCode
categories: LeetCode
---
## 第193场周赛

本周题目AC 2/4，dp相关算法还是要多加练习，最近疫情变严重了，要回学校了，注意防护。


### [5436. 一维数组的动态和](https://leetcode-cn.com/problems/running-sum-of-1d-array/)

给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。

请返回 nums 的动态和。
<!--more-->
示例1：
```
输入：nums = [1,2,3,4]
输出：[1,3,6,10]
解释：动态和计算过程为 [1, 1+2, 1+2+3, 1+2+3+4] 。
```

示例2:
```
输入：nums = [1,1,1,1,1]
输出：[1,2,3,4,5]
解释：动态和计算过程为 [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1] 。
```

示例3：
```
输入：nums = [3,1,2,10,1]
输出：[3,4,6,16,17]
```
提示：
- 1 <= nums.length <= 1000
- -10^6 <= nums[i] <= 10^6
### 思路

其实就是求前缀和，补充一句前缀和的一个作用是可这段数组任意区间内之和，

### 代码
```c++
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n,0);
        res[0] = nums[0];
        for (int i=1;i<n;i++){
            res[i] = nums[i] + res[i-1];
        }
        return res;
    }
};
```

### [5437. 不同整数的最少数目](https://leetcode-cn.com/problems/least-number-of-unique-integers-after-k-removals/)

给你一个整数数组 arr 和一个整数 k 。现需要从数组中恰好移除 k 个元素，请找出移除后数组中不同整数的最少数目。

示例 1：
```
输入：arr = [5,5,4], k = 1
输出：1
解释：移除 1 个 4 ，数组中只剩下 5 一种整数。
```

示例 2：
```
输入：arr = [4,3,1,1,3,3,2], k = 3
输出：2
解释：先移除 4、2 ，然后再移除两个 1 中的任意 1 个或者三个 3 中的任意 1 个，最后剩下 1 和 3 两种整数。
```

提示：
- 1 <= arr.length <= 10^5
- 1 <= arr[i] <= 10^9
- 0 <= k <= arr.length

### 思路
排序+贪心  O(nlogn)

先统计所有数的个数，然后按照出现的次数排序，然后统计最多可以移走多少种不同的数。

### 代码
```c++
class Solution {
public:
    int findLeastNumOfUniqueInts(vector<int>& arr, int k) {
        int res = 0;
        int n = arr.size();
        if(k>=n) return res;
        unordered_map<int,int> m;
        for (int &a : arr) {
            m[a]++;
        }
        vector<int> v;
        for (auto &p: m) {
            v.push_back(p.second);
        }
        sort(v.begin(),v.end());
        res = v.size();
        for (int &c: v) {
            if (k>=c) {
                k-=c;
                res--;
            }
            else {
                break;
            }
        }
        return res;
    }
};
```

### [5438. 制作 m 束花所需的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-make-m-bouquets/)

给你一个整数数组 bloomDay，以及两个整数 m 和 k 。

现需要制作 m 束花。制作花束时，需要使用花园中 相邻的 k 朵花 。

花园中有 n 朵花，第 i 朵花会在 bloomDay[i] 时盛开，恰好 可以用于 一束 花中。

请你返回从花园中摘 m 束花需要等待的最少的天数。如果不能摘到 m 束花则返回 -1 。

示例 1：
```
输入：bloomDay = [1,10,3,10,2], m = 3, k = 1
输出：3
解释：让我们一起观察这三天的花开过程，x 表示花开，而 _ 表示花还未开。
现在需要制作 3 束花，每束只需要 1 朵。
1 天后：[x, _, _, _, _]   // 只能制作 1 束花
2 天后：[x, _, _, _, x]   // 只能制作 2 束花
3 天后：[x, _, x, _, x]   // 可以制作 3 束花，答案为 3
```

示例 2：
```
输入：bloomDay = [1,10,3,10,2], m = 3, k = 2
输出：-1
解释：要制作 3 束花，每束需要 2 朵花，也就是一共需要 6 朵花。而花园中只有 5 朵花，无法满足制作要求，返回 -1 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-number-of-days-to-make-m-bouquets
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```
示例 3：
```
输入：bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
输出：12
解释：要制作 2 束花，每束需要 3 朵。
花园在 7 天后和 12 天后的情况如下：
7 天后：[x, x, x, x, _, x, x]
可以用前 3 朵盛开的花制作第一束花。但不能使用后 3 朵盛开的花，因为它们不相邻。
12 天后：[x, x, x, x, x, x, x]
显然，我们可以用不同的方式制作两束花。
```

示例 4：
```c++
输入：bloomDay = [1000000000,1000000000], m = 1, k = 1
输出：1000000000
解释：需要等 1000000000 天才能采到花来制作花束
```
示例 5：
```c++
输入：bloomDay = [1,10,2,9,3,8,4,7,5,6], m = 4, k = 2
输出：9
```

提示：
- bloomDay.length == n
- 1 <= n <= 10^5
- 1 <= bloomDay[i] <= 10^9
- 1 <= m <= 10^6
- 1 <= k <= n

### 思路

此题比赛时没有思路，好好学习一下。
1. 二分法，二分的遍历花开的天数，对于每一个天数遍历是否能摘到m束花
2. 滑动窗口，维护一个连续的动态区间[参考这里](https://www.bilibili.com/video/BV19K4y1x7sA)

### 代码
二分：
- 如果mid天满足，那么mid+1天也满足，满足单调性，可以二分check
- 对于当前天数mid，贪心计算有多少个连续的小于等于mid的k天（也就是开的花朵数），假设有cnt个连续k天，判断cnt是否大于等于m即可
```c++
class Solution {
public:
    bool check(vector<int>&a,int m,int k,int mid){
        int n=a.size();
        int cnt=0;  // 当前凑成的花束
        int len=0;  // 花朵数
        for(int i=0;i<n;i++){
            // 记录连续的开花数
            if(a[i]<=mid){
                len++;
                // 连续开够k朵凑够一束花束
                if(len==k){
                    cnt++;
                    len-=k;
                }
            }
            // 重新记录
            else{
                len=0;
            }
        }
        // 判断当前天数凑够的花束是否满足需求
        return cnt>=m;
    }
    int minDays(vector<int>& a, int m, int k) {
        int n=a.size();
        int l = *min_element(a.begin(),a.end());
        int r = *max_element(a.begin(),a.end());
        int res=-1;
        while(l<=r){
            int mid=l + (r-l)/2;
            if(check(a,m,k,mid)){
                res=mid;
                r=mid-1;
            }else{
                l=mid+1;
            }
        }
        return res;
    }
};
```

滑动区间
```c++
class Solution {
public:
    int get (int l, int r ,int k) {
        return (r-l+1)/k; // 向下取整
    }
    int minDays(vector<int>& bloomDay, int m, int k) {
        int res = -1;
        int n = bloomDay.size();
        if (n < m*k) return -1;
        vector<pair<int,int>> q;
        for (int i=0;i<n;i++) q.push_back({bloomDay[i],i+1});//下标从1开始
        sort(q.begin(),q.end());
        vector<int> l(n+2,0), r(n+2,0); // 1~n+1的下标
        // l[i] 表示 i为右端点，l[i]为左端点的区间，
        // r[i] 表示 i为左端点，r[i]为右端点的区间
        int sum = 0;
        for (auto x: q){
            int i = x.second;
            // 维护一个动态区间
            if (l[i-1] && r[i+1]) {  //左右可以合并成一个区间
                sum = sum - get(l[i-1],i-1,k) - get(i+1,r[i+1],k) + get(l[i-1],r[i+1],k);
                l[r[i+1]] = l[i-1];
                r[l[i-1]] = r[i+1];
            }
            else if (l[i-1]) { // 可以和左边合并到一个区间
                sum = sum - get(l[i-1],i-1,k) + get(l[i-1],i,k);
                r[l[i-1]] = i;
                l[i] = l[i-1];
            }
            else if (r[i+1]){ // 可以和右边边合并到一个区间
                sum = sum - get(i+1,r[i+1],k) + get(i, r[i+1],k);
                // 更新区间
                l[r[i+1]] = i;
                r[i] = r[i+1];
            }
            else {
                sum = sum + get(i,i,k);
                l[i] = r[i] = i;
            }
            if (sum>=m) return x.first;
        }
        return -1;
    }
};
```

### [5188. 树节点的第 K 个祖先](https://leetcode-cn.com/problems/kth-ancestor-of-a-tree-node/)

给你一棵树，树上有 n 个节点，按从 0 到 n-1 编号。树以父节点数组的形式给出，其中 parent[i] 是节点 i 的父节点。树的根节点是编号为 0 的节点。

请你设计并实现 getKthAncestor(int node, int k) 函数，函数返回节点 node 的第 k 个祖先节点。如果不存在这样的祖先节点，返回 -1 。

树节点的第 k 个祖先节点是从该节点到根节点路径上的第 k 个节点。

示例 ：
![](1.png)
```
输入：
["TreeAncestor","getKthAncestor","getKthAncestor","getKthAncestor"]
[[7,[-1,0,0,1,1,2,2]],[3,1],[5,2],[6,3]]

输出：
[null,1,0,-1]

解释：
TreeAncestor treeAncestor = new TreeAncestor(7, [-1, 0, 0, 1, 1, 2, 2]);

treeAncestor.getKthAncestor(3, 1);  // 返回 1 ，它是 3 的父节点
treeAncestor.getKthAncestor(5, 2);  // 返回 0 ，它是 5 的祖父节点
treeAncestor.getKthAncestor(6, 3);  // 返回 -1 因为不存在满足要求的祖先节点
```

提示：
- 1 <= k <= n <= 5*10^4
- parent[0] == -1 表示编号为 0 的节点是根节点。
- 对于所有的 0 < i < n ，0 <= parent[i] < n 总成立
- 0 <= node < n
- 至多查询 5*10^4 次

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