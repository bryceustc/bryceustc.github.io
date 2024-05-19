---
title: C++ 实现atoi 函数
date: 2024-05-19 15:06:45
tags: C++
categories: C++
---
实现c++函数库中atoi（）函数，要考虑到各种特殊情况：
- 空字符串。
- +和-号。
- 字符串前中后n个空格。
- 溢出。
- 非数字字符。

<!--more-->
```c++
#include <bits/stdc++.h>

using namespace std;

int MyAtoi (const string & str) {
    if (str.empty()) return 0;
    int n = str.size();
    int idx = 0;
    // 去除空格
    while (idx < n && isspace(str[idx])) idx++;
    // 正负数标记
    bool sign = true;
    if (str[idx] == '-') {
        sign = false;
        idx++;
    } else if (str[idx] == '+') {
        sign = true;
        idx++;
    }
    int res = 0;
    while (idx < n && isdigit(str[idx])) {
        int num = str[idx] - '0';
        res = res * 10 + num;
        idx++;
        if (res > std::numeric_limits<int>::max()) {
            if (sign) {
                return std::numeric_limits<int>::max();
            }
            else {
                return std::numeric_limits<int>::min();
            }
        }
    }
    return res;
}

int main() {
    string a = "    -0001234";
    string b = "   +4321";
    std::cout << "a: " << MyAtoi(a) << std::endl; // a: 1234
    std::cout << "b: " << MyAtoi(b) << std::endl; // b: 4321
    return 0;
}
```