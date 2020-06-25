---
title: C++”字母数字”判断相关的函数
date: 2020-06-25 17:52:37
tags: LeetCode
categories: LeetCode
---
LeetCode中做题遇到一个，需要解决数字、小写字母、大写字母的问题，可以用C++内置的函数，整理一下，做一下记录：

1. islower(char c) 是否为小写字母
2. isupper(char c) 是否为大写字母
3. isdigit(char c) 是否为数字
4. isalpha(char c) 是否为字母
5. isalnum(char c) 是否为字母或者数字
6. toupper(char c) 字母小转大
7. tolower(char c) 字母大转小

<!--more-->

### 题目描述:  验证回文串

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

**示例 1:**
```
输入: "A man, a plan, a canal: Panama"
输出: true
```
**示例 2:**
```
输入: "race a car"
输出: false
```

###  解法：双指针
```c++
class Solution {
public:
    bool isPalindrome(string s) {
        string tmp;
        for (const char& c : s) {
            if (islower(c) || isdigit(c)) {
                tmp += c;
            }
            else if (isupper(c)) {
                tmp += (c+32);  // tmp += tolower(c);
            }
        }
        int n = tmp.size();
        int i = 0;
        int j = n - 1;
        while (i < j) {
            if (tmp[i]!=tmp[j]) {
                return false;
            }
            else {
                i++;
                j--;
            }
        }
        return true;
    }
};
```
![](1.jpg)