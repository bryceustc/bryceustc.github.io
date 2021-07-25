---
title: 完全二叉树DFS转BFS
date: 2021-05-09 17:31:34
tags: 笔试
categories: 笔试
mathjax: true
---
# 题目描述:完全二叉树DFS转BFS

## 题目：
给定一个字符串，其字符序列是根据一个完全二叉树通过深度优先遍历得到的，二叉树的每个节点是一个单字符。如果对这个完全二叉树改用广度优先遍历算法，那么得到的字符串是什么，将这个字符串序列以字符串的形式输出。

输入描述：

输入一行，一个由字母和数字组成的字符串，字符串长度为2**n-1，**表示次方，例如 2 ** 3-1 = 7，n大于等于1。如果输入字符串的长度不合要求，那么统一输出字符串WRONG INPUT。

输出描述：
输出一行字符串。

样例输入：abcdefg
样例输出：abecdfg
 <!--more--> 
# 本题考点：
  
  1). 完全二叉树的理解
  
  2). 前序遍历

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;
int n, i;
string x, res;

bool Check(int n) {
    if ((n&(n-1))==0) {
        return false;
    }
    return true;
}

void dfs(int index) {
    if(index < n) {
        res[index] = x[i++];
        dfs(index * 2);
        dfs(index * 2 + 1);
    }
}
int main() {
    cin>>x;
    n = x.size() + 1;
    if (Check(n)) {
        cout << "WRONG INPUT";
    } else {
        res = x;
        i = 0;
        dfs(1);
        for (int i = 1; i < n; i++)
            cout << res[i];
    }
    return 0;
}
```
