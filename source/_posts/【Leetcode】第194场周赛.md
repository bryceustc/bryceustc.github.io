---
title: 【Leetcode】第194场周赛
date: 2020-06-21 13:17:57
tags: LeetCode
categories: LeetCode
---
## 第194场周赛

本周题目AC 2/4，本周题目有点难度感觉做的不顺，在遇到选取集合存储的时候，需要考虑是要进行什么操作，再选取和是的容器，比如第三题，考虑到插入和选取最小值可以选用小顶堆来实现。


### [5440. 数组异或操作](https://leetcode-cn.com/contest/weekly-contest-194/problems/xor-operation-in-an-array/)

给你两个整数，n 和 start 。

数组 nums 定义为：nums[i] = start + 2*i（下标从 0 开始）且 n == nums.length 。

请返回 nums 中所有元素按位异或（XOR）后得到的结果。
<!--more-->
示例1：
```
输入：n = 5, start = 0
输出：8
解释：数组 nums 为 [0, 2, 4, 6, 8]，其中 (0 ^ 2 ^ 4 ^ 6 ^ 8) = 8 。
     "^" 为按位异或 XOR 运算符。
```

示例2:
```
输入：n = 4, start = 3
输出：8
解释：数组 nums 为 [3, 5, 7, 9]，其中 (3 ^ 5 ^ 7 ^ 9) = 8.
```

示例3：
```
输入：nums = [3,1,2,10,1]
输出：[3,4,6,16,17]
```
提示：
- 1 <= n <= 1000
- 0 <= start <= 1000
- n == nums.length
### 思路

直接模拟

### 代码
```c++
class Solution {
public:
    int xorOperation(int n, int start) {
        vector<int> nums(n);
        int res = 0;
        for (int i=0;i<n;i++) {
            nums[i] = start + 2*i;
            res ^= nums[i];
        }
        return res;
    }
};
```

### [5441. 保证文件名唯一](https://leetcode-cn.com/contest/weekly-contest-194/problems/making-file-names-unique/)

给你一个长度为 n 的字符串数组 names 。你将会在文件系统中创建 n 个文件夹：在第 i 分钟，新建名为 names[i] 的文件夹。

由于两个文件 不能 共享相同的文件名，因此如果新建文件夹使用的文件名已经被占用，系统会以 (k) 的形式为新文件夹的文件名添加后缀，其中 k 是能保证文件名唯一的 最小正整数 。

返回长度为 n 的字符串数组，其中 ans[i] 是创建第 i 个文件夹时系统分配给该文件夹的实际名称。

示例 1：
```
输入：names = ["pes","fifa","gta","pes(2019)"]
输出：["pes","fifa","gta","pes(2019)"]
解释：文件系统将会这样创建文件名：
"pes" --> 之前未分配，仍为 "pes"
"fifa" --> 之前未分配，仍为 "fifa"
"gta" --> 之前未分配，仍为 "gta"
"pes(2019)" --> 之前未分配，仍为 "pes(2019)"
```

示例 2：
```
输入：names = ["gta","gta(1)","gta","avalon"]
输出：["gta","gta(1)","gta(2)","avalon"]
解释：文件系统将会这样创建文件名：
"gta" --> 之前未分配，仍为 "gta"
"gta(1)" --> 之前未分配，仍为 "gta(1)"
"gta" --> 文件名被占用，系统为该名称添加后缀 (k)，由于 "gta(1)" 也被占用，所以 k = 2 。实际创建的文件名为 "gta(2)" 。
"avalon" --> 之前未分配，仍为 "avalon"
```
示例3：
```
输入：names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]
输出：["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]
解释：当创建最后一个文件夹时，最小的正有效 k 为 4 ，文件名变为 "onepiece(4)"。
```
示例4：
```
输入：names = ["wano","wano","wano","wano"]
输出：["wano","wano(1)","wano(2)","wano(3)"]
解释：每次创建文件夹 "wano" 时，只需增加后缀中 k 的值即可。
```
示例5：
```
输入：names = ["kaido","kaido(1)","kaido","kaido(1)"]
输出：["kaido","kaido(1)","kaido(2)","kaido(1)(1)"]
解释：注意，如果含后缀文件名被占用，那么系统也会按规则在名称后添加新的后缀 (k) 。
```

提示：
- 1 <= names.length <= 5 * 10^4
- 1 <= names[i].length <= 20
- names[i] 由小写英文字母、数字和/或圆括号组成。

### 思路
巧妙利用哈希，利用哈希表存储，key：已经出现过的名字，val：出现的次数

### 代码
```c++
class Solution {
public:
    vector<string> getFolderNames(vector<string>& names) {
        int n = names.size();
        vector<string> res;
        unordered_map<string, int> m;
        for (int i = 0; i < n; i++) {
            string s = names[i];
            string t;
            // 如果是新文件，直接添加
            if (!m.count(s)) {
                res.push_back(s);
            }
            else {
                // 如果出现过，就更新
                t = s + "(" + to_string(m[s]) + ")";
                // 判断更新过的有没有出现过，有再继续更新
                while(m.count(t)) {
                    m[s]++;
                    t = s + "(" + to_string(m[s]) + ")";
                }
                m[t]++;
                res.push_back(t);
            }
            m[s]++;
        }
        return res;
    }
};
```

### [5442. 避免洪水泛滥](https://leetcode-cn.com/contest/weekly-contest-194/problems/avoid-flood-in-the-city/)

你的国家有无数个湖泊，所有湖泊一开始都是空的。当第 n 个湖泊下雨的时候，如果第 n 个湖泊是空的，那么它就会装满水，否则这个湖泊会发生洪水。你的目标是避免任意一个湖泊发生洪水。

给你一个整数数组 rains ，其中：

- rains[i] > 0 表示第 i 天时，第 rains[i] 个湖泊会下雨。
- rains[i] == 0 表示第 i 天没有湖泊会下雨，你可以选择 一个 湖泊并 抽干 这个湖泊的水。
请返回一个数组 ans ，满足：

- ans.length == rains.length
- 如果 rains[i] > 0 ，那么ans[i] == -1 。
- 如果 rains[i] == 0 ，ans[i] 是你第 i 天选择抽干的湖泊。
如果有多种可行解，请返回它们中的 任意一个 。如果没办法阻止洪水，请返回一个 空的数组 。

请注意，如果你选择抽干一个装满水的湖泊，它会变成一个空的湖泊。但如果你选择抽干一个空的湖泊，那么将无事发生（详情请看示例 4）。

示例 1：
```
输入：rains = [1,2,3,4]
输出：[-1,-1,-1,-1]
解释：第一天后，装满水的湖泊包括 [1]
第二天后，装满水的湖泊包括 [1,2]
第三天后，装满水的湖泊包括 [1,2,3]
第四天后，装满水的湖泊包括 [1,2,3,4]
没有哪一天你可以抽干任何湖泊的水，也没有湖泊会发生洪水。
```

示例 2：
```
输入：rains = [1,2,0,0,2,1]
输出：[-1,-1,2,1,-1,-1]
解释：第一天后，装满水的湖泊包括 [1]
第二天后，装满水的湖泊包括 [1,2]
第三天后，我们抽干湖泊 2 。所以剩下装满水的湖泊包括 [1]
第四天后，我们抽干湖泊 1 。所以暂时没有装满水的湖泊了。
第五天后，装满水的湖泊包括 [2]。
第六天后，装满水的湖泊包括 [1,2]。
可以看出，这个方案下不会有洪水发生。同时， [-1,-1,1,2,-1,-1] 也是另一个可行的没有洪水的方案。
```
示例 3：
```
输入：rains = [1,2,0,1,2]
输出：[]
解释：第二天后，装满水的湖泊包括 [1,2]。我们可以在第三天抽干一个湖泊的水。
但第三天后，湖泊 1 和 2 都会再次下雨，所以不管我们第三天抽干哪个湖泊的水，另一个湖泊都会发生洪水。
```

示例 4：
```c++
输入：rains = [69,0,0,0,69]
输出：[-1,69,1,1,-1]
解释：任何形如 [-1,69,x,y,-1], [-1,x,69,y,-1] 或者 [-1,x,y,69,-1] 都是可行的解，其中 1 <= x,y <= 10^9
```
示例 5：
```c++
输入：rains = [10,20,20]
输出：[]
解释：由于湖泊 20 会连续下 2 天的雨，所以没有没有办法阻止洪水。
```

提示：
- 1 <= rains.length <= 10^5
- 0 <= rains[i] <= 10^9

### 思路

贪心来做，比赛时是利用了一个哈希来做，哈希表存储下过雨的湖泊，遇到rains[i]=0,就清理之后最近的一个要下雨的湖泊，TLE了

最优解法：
记录每次下雨之后，同一个湖下一次下雨是什么时候，然后每次抽水抽"当前有雨的湖里面，下一次下雨最早的湖"
### 代码
```c++
class Solution {
public:
    vector<int> avoidFlood(vector<int>& rains) {
        int n = rains.size();
        vector<int> res(n);
        unordered_map<int,int> m;
        for (int i=0;i<n;i++) {
            if (rains[i]>0) {
                if (m.count(rains[i])) return {};
                else {
                    res[i] = -1;
                    m[rains[i]]++;
                }
            }
            else if (rains[i]==0) {
                if (!m.empty()){
                    int j = i+1;
                    for (; j<n;j++) {
                        if (m.count(rains[j])) {
                            res[i] = rains[j];
                            m.erase(rains[j]);
                            break;
                        }
                    }
                    if (j==n) {
                        res[i] = m.begin()->first;
                        m.erase(m.begin());
                    }
                }
                else {
                    res[i] =1;
                }
            }
        }
        return res;
    }
};
```


```c++
class Solution {
public:
    vector<int> avoidFlood(vector<int>& rains) {
        int n = rains.size();
        vector<int> res;
        unordered_map<int,int> m;
        // next记录某一个湖泊下一次下雨的时间，注意要倒着遍历
        vector<int> next(n,n+1);
        for (int i=n-1;i>=0;i--) {
            if (m.count(rains[i])) {
                next[i] = m[rains[i]];
            }
            else {
                m[rains[i]] = i;
            }
        }
        typedef pair<int,int> P;
        priority_queue<P, vector<P>, greater<P>> heap;
        unordered_set<int> s;
        for (int i=0;i<n;i++) {
            int r = rains[i];
            // r>0 下雨
            if (r) {
                // 湖泊r是满的，再下雨就return{}
                if (s.count(r)) return {};
                // 不是满的， 就用哈希表存储一下
                s.insert(r);
                heap.push({next[i], r}); // 将该湖泊下次下雨的时间以及湖泊编号存入小顶堆
                res.push_back(-1);
            }
            // rains[i] = 0 需要抽干一个湖泊
            else {
                // 没有湖泊是满的，就随机抽取一个湖泊，这里设置为1
                if (heap.empty()) {
                    res.push_back(1);
                }
                // 需要抽取之后最先下雨的湖泊，就从小顶堆里选取
                else {
                    auto t = heap.top();
                    heap.pop();
                    s.erase(t.second);
                    res.push_back(t.second);
                }
            }
        }
        return res;
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