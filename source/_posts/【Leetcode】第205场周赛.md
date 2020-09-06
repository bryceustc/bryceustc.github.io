---
title: 【Leetcode】第205场周赛
date: 2020-09-06 15:06:46
vtags: LeetCode
categories: LeetCode
mathjax: true
---

第205场周赛，最近忙着找工作，周日的周赛还是按照惯例都参加一下吧，自己非科班，想要找到好工作，真的要更加加油啊，补基础，多刷题，提升coding能力！

### [5507. 替换所有的问号](https://leetcode-cn.com/contest/weekly-contest-205/problems/replace-all-s-to-avoid-consecutive-repeating-characters/)

给你一个仅包含小写英文字母和 '?' 字符的字符串 s<var> </var>，请你将所有的 '?' 转换为若干小写字母，使最终的字符串不包含任何 连续重复 的字符。

注意：你 不能 修改非 '?' 字符。

题目测试用例保证 除 '?' 字符 之外，不存在连续重复的字符。

在完成所有转换（可能无需转换）后返回最终的字符串。如果有多个解决方案，请返回其中任何一个。可以证明，在给定的约束条件下，答案总是存在的。

<!--more-->
示例1：
```
输入：s = "?zs"
输出："azs"
解释：该示例共有 25 种解决方案，从 "azs" 到 "yzs" 都是符合题目要求的。只有 "z" 是无效的修改，因为字符串 "zzs" 中有连续重复的两个 'z' 。
```

示例2:
```
输入：s = "ubv?w"
输出："ubvaw"
解释：该示例共有 24 种解决方案，只有替换成 "v" 和 "w" 不符合题目要求。因为 "ubvvw" 和 "ubvww" 都包含连续重复的字符。
```

示例3：
```
输入：s = "j?qg??b"
输出："jaqgacb"
```

示例4：
```
输入：s = "??yw?ipkj?"
输出："acywaipkja"
```

提示：
- 1 <= s.length <= 100
- s 仅包含小写英文字母和 '?' 字符
### 思路

出现问号的地方就直接去判断，枚举字母，与前边和后边相邻的位置判断。

### 代码
```c++
class Solution {
public:
    string modifyString(string s) {
        int n = s.size();
        for (int i = 0; i < n; i++) {
            if (s[i] == '?') {
                char c = 'a';
                // 如果左边和右边有一个不满足就继续枚举。
                while ((i > 0 && s[i-1] == c) || (i < n - 1 && s[i+1] == c) ) c++;
                s[i] = c;
            }
        }
        return s;
    }
};
```

### [5508. 数的平方等于两数乘积的方法数](https://leetcode-cn.com/contest/weekly-contest-205/problems/number-of-ways-where-square-of-number-is-equal-to-product-of-two-numbers/)

给你两个整数数组 nums1 和 nums2 ，请你返回根据以下规则形成的三元组的数目（类型 1 和类型 2 ）：

类型 1：三元组 (i, j, k) ，如果 nums1[i]2 == nums2[j] * nums2[k] 其中 0 <= i < nums1.length 且 0 <= j < k < nums2.length
类型 2：三元组 (i, j, k) ，如果 nums2[i]2 == nums1[j] * nums1[k] 其中 0 <= i < nums2.length 且 0 <= j < k < nums1.length

示例 1：

```
输入：nums1 = [7,4], nums2 = [5,2,8,9]
输出：1
解释：类型 1：(1,1,2), nums1[1]^2 = nums2[1] * nums2[2] (4^2 = 2 * 8)
```

示例 2：
```
输入：nums1 = [1,1], nums2 = [1,1,1]
输出：9
解释：所有三元组都符合题目要求，因为 1^2 = 1 * 1
类型 1：(0,0,1), (0,0,2), (0,1,2), (1,0,1), (1,0,2), (1,1,2), nums1[i]^2 = nums2[j] * nums2[k]
类型 2：(0,0,1), (1,0,1), (2,0,1), nums2[i]^2 = nums1[j] * nums1[k]
```

示例 3:
```
输入：nums1 = [7,7,8,3], nums2 = [1,2,9,7]
输出：2
解释：有两个符合题目要求的三元组
类型 1：(3,0,2), nums1[3]^2 = nums2[0] * nums2[2]
类型 2：(3,0,1), nums2[3]^2 = nums1[0] * nums1[1]
```

示例 4：
```
输入：nums1 = [4,7,9,11,23], nums2 = [3,5,1024,12,18]
输出：0
解释：不存在符合题目要求的三元组
```

提示：
- 1 <= nums1.length, nums2.length <= 1000
- 1 <= nums1[i], nums2[i] <= 10^5

### 思路

比赛时，暴力枚举了，然后超时了3个测试用例。

首先上来先注意数据范围，1000，一般是n^2或者n^2*logn，可以利用哈希来存储平方结果，然后遍历j，k，达到n^2的时间复杂度。

### 代码
```c++
typedef long long ll;

class Solution {
public:
    
    int helper(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<ll, int> m;
        int res = 0;
        for (int a : nums1) {
            m[(ll) a * a]++;
        }
        int n = nums2.size();
        for (int j = 0; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                res += m[(ll)nums2[j] * nums2[k]];
            }
        }
        return res;
    }
    
    int numTriplets(vector<int>& nums1, vector<int>& nums2) {
        return helper(nums1, nums2) + helper(nums2, nums1);
    }
};
```

### [5509. 避免重复字母的最小删除成本](https://leetcode-cn.com/contest/weekly-contest-205/problems/minimum-deletion-cost-to-avoid-repeating-letters/)

给你一个字符串 s 和一个整数数组 cost ，其中 cost[i] 是从 s 中删除第 i 个字符的代价。

返回使字符串任意相邻两个字母不相同的最小删除成本。

请注意，删除一个字符后，删除其他字符的成本不会改变。

示例 1：
```
输入：s = "abaac", cost = [1,2,3,4,5]
输出：3
解释：删除字母 "a" 的成本为 3，然后得到 "abac"（字符串中相邻两个字母不相同）。
```

示例 2：
```
输入：s = "abc", cost = [1,2,3]
输出：0
解释：无需删除任何字母，因为字符串中不存在相邻两个字母相同的情况。
```

示例 3:
```
输入：s = "aabaa", cost = [1,2,3,4,1]
输出：2
解释：删除第一个和最后一个字母，得到字符串 ("aba") 。
```

提示：
- s.length == cost.length
- 1 <= s.length, cost.length <= 10^5
- 1 <= cost[i] <= 10^4
- s 中只含有小写英文字母

### 思路
1. 贪心，滑动，一段连续相同字符的区间，就只保留最大的cost，剩下的删掉

2. dp 

定义状态：dp[i][k] 考虑了前i个字符（可能删除了一些）满足了题目要求，最后一个字符现在是k的最小代价是多少。

转移方程：

对于 s[i] = c

不删掉: dp[i][c] <- dp[i-1][k] 并且 k != c

删掉: dp[i][k] <- dp[i-1][k] + cost[i] k为任何字符都可以

### 代码

贪心
```c++
class Solution {
public:
    int minCost(string s, vector<int>& cost) {
        int res = 0;
        int n = s.size();
        char c =s[0];
        int l = 0;
        int r = 0;
        for (int i = 1; i < n; i++) {
            if (s[i] == c) {
                r++;
            } else {
                c = s[i];
                res += helper(cost, l, r);
                l = i;
                r = i;
            }
        }
        res += helper(cost, l, r);
        return res;
    }
    
    int helper(vector<int> &cost, int l, int r) {
        int  sum = 0;
        if ( l == r) return sum;
        int a = INT_MIN;
        for (int i = l; i <=r; i++) {
            sum += cost[i];
            a = max(cost[i], a);
        }
        sum -= a;
        return sum;
    }
};
```

dp
```c++
const int MAXN = 1e5 + 50;
int dp[MAXN][30];

class Solution {
public:
    int minCost(string s, vector<int>& cost) {
        int n = s.length();
        // 初始化dp数组
        for (int i = 0; i <= n; i++) for (int k = 0; k < 26; k++) dp[i][k] = -1;
        for (int k = 0; k < 26; k++) dp[0][k] = 0;
        
        for (int i = 1; i <= n; i++){
            int c = s[i - 1] - 'a', v = cost[i - 1];
            // 不删掉的情况
            for (int k = 0; k < 26; k++){
                if (k == c) continue;
                if (dp[i - 1][k] == -1) continue;
                if (dp[i][c] == -1 || dp[i][c] > dp[i - 1][k]) dp[i][c] = dp[i - 1][k];    
            }

            // 删掉的情况
            for (int k = 0; k < 26; k++){
                if (dp[i - 1][k] == -1) continue;
                if (dp[i][k] == -1 || dp[i][k] > dp[i - 1][k] + v) dp[i][k] = dp[i - 1][k] + v;
            }
        }
        
        int ans = -1;
        for (int k = 0; k < 26; k++){
            if (dp[n][k] == -1) continue;
            if (ans == -1 || ans > dp[n][k]) ans = dp[n][k];
        }
        return ans;
    }
};
```


### [5510. 保证图可完全遍历](https://leetcode-cn.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/)

Alice 和 Bob 共有一个无向图，其中包含 n 个节点和 3  种类型的边：

类型 1：只能由 Alice 遍历。
类型 2：只能由 Bob 遍历。
类型 3：Alice 和 Bob 都可以遍历。
给你一个数组 edges ，其中 edges[i] = [typei, ui, vi] 表示节点 ui 和 vi 之间存在类型为 typei 的双向边。请你在保证图仍能够被 Alice和 Bob 完全遍历的前提下，找出可以删除的最大边数。如果从任何节点开始，Alice 和 Bob 都可以到达所有其他节点，则认为图是可以完全遍历的。

返回可以删除的最大边数，如果 Alice 和 Bob 无法完全遍历图，则返回 -1 。

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/06/5510ex1.png)
```
输入：n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
输出：2
解释：如果删除 [1,1,2] 和 [1,1,3] 这两条边，Alice 和 Bob 仍然可以完全遍历这个图。再删除任何其他的边都无法保证图可以完全遍历。所以可以删除的最大边数是 2 。
```

示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/06/5510ex2.png)
```
输入：n = 4, edges = [[3,1,2],[3,2,3],[1,1,4],[2,1,4]]
输出：0
解释：注意，删除任何一条边都会使 Alice 和 Bob 无法完全遍历这个图。
```

示例3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/06/5510ex3.png)
```
输入：n = 4, edges = [[3,2,3],[1,1,2],[2,3,4]]
输出：-1
解释：在当前图中，Alice 无法从其他节点到达节点 4 。类似地，Bob 也不能达到节点 1 。因此，图无法完全遍历。
```

提示：
- 1 <= n <= 10^5
- 1 <= edges.length <= min(10^5, 3 * n * (n-1) / 2)
- edges[i].length == 3
- 1 <= edges[i][0] <= 3
- 1 <= edges[i][1] < edges[i][2] <= n
- 所有元组 (typei, ui, vi) 互不相同

### 思路
首先判断Alice和Bob的是不是连通图

1.考虑单个用户，共有n个结点，产生连通图需要的边数为n-1,假设该用户可以访问的边有m条，那么多余的为m-(n-1)

2.现在有两个用户，结点为n个，两个用户各自可以访问的边数为p,q（不包含第三种类型的边）,其中第三种类型的边为K1条

假设最终的连通图用了K2条第三种类型的边（无多余的），有K2<=K1,则有：

a.对于第一个用户，多余的边为p-(n-1-K2),其中n-1-K2为，对于第一个用户，还需要多少条只有第一个用户可以访问的边才能构成连通图

b.对于第二个用户，多余的边为q-(n-1-K2)，其中n-1-K2为，对于第二个用户，还需要多少条只有第二个用户可以访问的边才能构成连通图

那么总的多余边(需删除的)为p-(n-1-K2)+q-(n-1-K2)+(K1-K2) = p+q-2n+2+K1+K2;

由于K1,p,q,n为定数，所以要想删除的边最多，那么必须要求最终的图中第三种类型的边最多，且无多余

因此，可以先添加第三条边，先在第三条边中去除多余的，然后再在剩下的各自可以访问的边中去除多余的

### 代码
```c++
class Solution {
public:
    int getRoot(vector<int>& par,int x){
        int root = x;
        while(par[root]!=root){
            root = par[root];
        }
        while(par[x]!=root){
            int tmp = par[x];
            par[x] = root;
            x = tmp;
        }
        return root;
    }
    bool merge(vector<int>& par,int x,int y){
        int _x = getRoot(par,x);
        int _y = getRoot(par,y);
        if(_x!=_y){
            par[_x]=_y;
            return true;
        }
        return false;
    }
    int maxNumEdgesToRemove(int n, vector<vector<int>>& edges) {
        vector<int>par1 = vector<int>(n+1,0);
        vector<int>par2;
        int ans = 0;
        int cnt1 = n,cnt2;
        for(int i =1;i<=n;i++){
            par1[i] = i;
        }
        //先添加第三种类型的边
        for(int i = 0;i<edges.size();i++){
            if(edges[i][0]==3){
                if(!merge(par1,edges[i][1],edges[i][2]))
                    ans++;
                else
                    cnt1--;
            }
        }
        par2 = par1;
        cnt2 = cnt1;
        //再添加其余两种类型的边
        for(int i = 0;i<edges.size();i++){
            if(edges[i][0]==1){
                if(!merge(par1,edges[i][1],edges[i][2]))
                    ans++;
                else
                    cnt1--;
            }else if(edges[i][0]==2){
                if(!merge(par2,edges[i][1],edges[i][2]))
                    ans++;
                else
                    cnt2--;
            }
        }
        if(cnt1!=1||cnt2!=1)
            return -1;
        return ans;
    }
};
```