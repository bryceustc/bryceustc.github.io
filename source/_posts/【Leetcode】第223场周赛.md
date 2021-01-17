---
title: 【Leetcode】第223场周赛
date: 2021-01-17 12:11:36
tags: LeetCode
categories: LeetCode
mathjax: true
---

第223场周赛

### [5649. 解码异或后的数组](https://leetcode-cn.com/problems/decode-xored-array/)

未知 整数数组 arr 由 n 个非负整数组成。

经编码后变为长度为 n - 1 的另一个整数数组 encoded ，其中 encoded[i] = arr[i] XOR arr[i + 1] 。例如，arr = [1,0,2,1] 经编码后得到 encoded = [1,2,3] 。

给你编码后的数组 encoded 和原数组 arr 的第一个元素 first（arr[0]）。

请解码返回原数组 arr 。可以证明答案存在并且是唯一的。

<!--more-->
示例1：
```
输入：encoded = [1,2,3], first = 1
输出：[1,0,2,1]
解释：若 arr = [1,0,2,1] ，那么 first = 1 且 encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]
```

示例2:
```
输入：encoded = [6,2,7,3], first = 4
输出：[4,2,0,7,4]
```

提示：
- 2 <= n <= 10^4
- encoded.length == n - 1
- 0 <= encoded[i] <= 10^5
- 0 <= first <= 10^5
### 思路
模拟法，利用异或的特性，a^b^a = b,一个数异或偶数次相当于直接去掉该数

### 代码
```c++
class Solution {
public:
    vector<int> decode(vector<int>& encoded, int first) {
        int n = encoded.size();
        vector<int> res(n+1);
        res[0] = first;
        for (int i = 1; i <= n; i++) {
            res[i] = encoded[i-1] ^ res[i-1];
        }
        return res;
    }
};
```

### [5652. 交换链表中的节点](https://leetcode-cn.com/problems/swapping-nodes-in-a-linked-list/)

给你链表的头节点 head 和一个整数 k。
交换 链表正数第k个节点和倒数第 k个节点的值后，返回链表的头节点（链表 从 1开始索引）

示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/10/linked1.jpg)
```
输入：head = [1,2,3,4,5], k = 2
输出：[1,4,3,2,5]
```

示例 2：
```
输入：head = [7,9,6,6,7,8,3,0,9,5], k = 5
输出：[7,9,6,6,8,7,3,0,9,5]
```

示例 3：
```
输入：head = [1], k = 1
输出：[1]
```

示例 4：
```
输入：head = [1,2], k = 1
输出：[2,1]
```

示例 5：
```
输入：head = [1,2,3], k = 2
输出：[1,2,3]
```

提示：
- 链表中节点的数目是 n
- 1 <= k <= n <= 10^5
- 0 <= Node.val <= 100

### 思路
分别找到第k个节点和第n-k个节点即可，然后将两个节点的值进行交换即可。

比赛当中遇到链表中的题，可以转换成数组来求。面试就算了
### 代码
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int length(ListNode * head){
        ListNode * curr = head;
        int ans = 0;
        while(curr){
            ans++;
            curr = curr->next;
        }
        
        return ans;
    }
    
    ListNode * find(ListNode * head,int k){
        ListNode * curr = head;
        while(curr && k > 1){
            curr = curr->next;
            k--;
        }
        return curr;
    }
    
    ListNode* swapNodes(ListNode* head, int k) {
        int n = length(head);
        ListNode * l1 = NULL;
        ListNode * l2 = NULL;
        l1 = find(head,k);
        l2 = find(head,n-k+1);
        swap(l1->val,l2->val);
        return head;
    }
};
```

比赛提交代码，真正意义上的交换节点
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapNodes(ListNode* head, int k) {
        if (head == NULL) return NULL;
        ListNode* dummyHead = new ListNode(-1);
        dummyHead->next = head;
        ListNode* p = dummyHead;
        ListNode* q = dummyHead;
        ListNode* pre_q;
        ListNode* pre_p;
        ListNode* res;
        int n = 0;
        while (p->next != NULL) {
            p = p->next;
            n++;
        }
        p = dummyHead;
        if (k > n) {
            res = dummyHead->next;
            return res;
        }
        for (int i = 1; i <= k; i++) {
            pre_q = q;
            q = q->next;
            
        }
        for (int j = 1; j <= n - k + 1; j++) {
            pre_p = p;
            p = p->next;
        }
        pre_q->next = p;
        pre_p->next = q;
        ListNode* tmp = q->next;
        q->next = p->next;
        p->next = tmp;
        res = dummyHead->next;
        return res;
    }
};
```

### [1722. 执行交换操作后的最小汉明距离](https://leetcode-cn.com/problems/minimize-hamming-distance-after-swap-operations/)

给你两个整数数组 source 和 target ，长度都是 n 。还有一个数组 allowedSwaps，其中每个 allowedSwaps[i] = [ai, bi]表示你可以交换数组 source 中下标为 ai 和 bi（下标从 0 开始）的两个元素。注意，你可以按 任意 顺序 多次 交换一对特定下标指向的元素。

相同长度的两个数组 source 和target 间的 汉明距离 是元素不同的下标数量。形式上，其值等于满足 source[i] != target[i]（下标从 0 开始）的下标 i（0 <= i <= n-1）的数量。

在对数组 source执行 任意 数量的交换操作后，返回 source和 target间的 最小汉明距离 。

示例 1：
```
输入：source = [1,2,3,4], target = [2,1,4,5], allowedSwaps = [[0,1],[2,3]]
输出：1
解释：source 可以按下述方式转换：
- 交换下标 0 和 1 指向的元素：source = [2,1,3,4]
- 交换下标 2 和 3 指向的元素：source = [2,1,4,3]
source 和 target 间的汉明距离是 1 ，二者有 1 处元素不同，在下标 3 。
```

示例 2：
```
输入：source = [1,2,3,4], target = [1,3,2,4], allowedSwaps = []
输出：2
解释：不能对 source 执行交换操作。
source 和 target 间的汉明距离是 2 ，二者有 2 处元素不同，在下标 1 和下标 2 。
```

示例3：
```
输入：source = [5,1,2,4,3], target = [1,5,4,2,3], allowedSwaps = [[0,4],[4,2],[1,3],[1,4]]
输出：0
```

提示：
- n == source.length == target.length
- 1 <= n <= 10^5
- 1 <= source[i], target[i] <= 10^5
- 0 <= allowedSwaps.length <= 10^5
- allowedSwaps[i].length == 2
- 0 <= ai, bi <= n - 1
- ai != bi

### 思路

并查集

1. 首先同属于并查集的两个元素，可以通过无限次的交换，总能交换到特定的位置。
2. 首先我们对allowedSwaps进行并查集合并，如果两个元素可以进行交换，则将其合并。将合并后的元素索引的集合进行遍历，同属于一个集合下面的元素的值依次加入集合，并进行统计。
3. 对target数组进行遍历，如果当前索引的i所在的集合中存在与target[i]相同的元素，则对其进行减1操作即可。

### 代码
```c++
class Solution {
public:
    int find(vector<int> & f,int x){
        while(f[x] != f[f[x]])  f[x] = f[f[x]];
        return f[x];
    }
    
    void uni(vector<int> & f,int x,int y){
        int x1 = find(f,x);
        int y1 = find(f,y);
        if(x1 != y1) f[x1] = y1;
    }
    
    int minimumHammingDistance(vector<int>& source, vector<int>& target, vector<vector<int>>& allowedSwaps) {
        int m = source.size();
        int n = allowedSwaps.size();
        int ans = 0;
        vector<int> f(m);
        unordered_map<int,map<int,int>> cnt;
        
        for(int i = 0; i < m; ++i) f[i] = i;
        for(auto v : allowedSwaps){
            uni(f,v[0],v[1]);
        }
        for(int i = 0; i < m; ++i){
            cnt[find(f,i)][source[i]]++;
        }
        for(int i = 0; i < m; ++i){
            int root = find(f,i);
            if(cnt[root].count(target[i])){
                cnt[root][target[i]]--;
                if(cnt[root][target[i]] == 0){
                    cnt[root].erase(target[i]);
                }
            }else{
                ans++;
            }
        }
        
        return ans;
    }
};
```

### [5639. 完成所有工作的最短时间](https://leetcode-cn.com/contest/weekly-contest-223/problems/find-minimum-time-to-finish-all-jobs/)

给你一个整数数组 jobs ，其中 jobs[i] 是完成第 i 项工作要花费的时间。

请你将这些工作分配给 k 位工人。所有工作都应该分配给工人，且每项工作只能分配给一位工人。工人的 工作时间 是完成分配给他们的所有工作花费时间的总和。请你设计一套最佳的工作分配方案，使工人的 最大工作时间 得以 最小化 。
返回分配方案中尽可能 最小 的 最大工作时间 。

示例 1：
```
输入：jobs = [3,2,3], k = 3
输出：3
解释：给每位工人分配一项工作，最大工作时间是 3 。
```

示例 2：
```
输入：jobs = [1,2,4,7,8], k = 2
输出：11
解释：按下述方式分配工作：
1 号工人：1、2、8（工作时间 = 1 + 2 + 8 = 11）
2 号工人：4、7（工作时间 = 4 + 7 = 11）
最大工作时间是 11 。
```

提示：
- 1 <= k <= jobs.length <= 12
- 1 <= jobs[i] <= 107

### 思路
dp + 子集遍历


### 代码
```c++
int dp[13][5000];
int cost[5000];
const int INF = 1e9 + 7;
class Solution {
public:
    int minimumTimeRequired(vector<int>& jobs, int k) {
        int n = jobs.size();
        int mask = (1<<n)-1;
        memset(dp,INF,sizeof(dp));
        memset(cost,0,sizeof(cost));
        
        /*intial*/
        dp[0][0] = 0;
        for(int i = 1; i <= k; ++i) dp[i][0] = 0;
        for(int i = 1; i < (1<<n); ++i){
            for(int j = 0; j < n; ++j){
                if(i&(1<<j)) cost[i] += jobs[j];
            }
            dp[1][i] = cost[i];
        }
        
        for(int i = 2; i <= k; ++i){
            for(int j = 1; j <= mask; ++j){
                for(int l = j; l != 0; l = (l-1)&j){
                    if(dp[i-1][j^l] != INF)
                        dp[i][j] = min(dp[i][j],max(dp[i-1][j^l],cost[l]));
                }
            }
        }
        
        return dp[k][(1<<n)-1];
    }
};
```