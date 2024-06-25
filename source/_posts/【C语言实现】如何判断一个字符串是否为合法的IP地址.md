---
title: 【C语言实现】如何判断一个字符串是否为合法的IP地址
date: 2024-06-25 22:22:14
tags: 字符串
categories: C++
---

### 题目：
如何判断一个字符串是否为合法的IP地址。

### 要求：
不能使用正则表达式和自带的库函数，以及string等各种类。

C++源码如下：

<!--more-->
```c
#include <bits/stdc++.h>


//ip字符串输入默认采用IPV4的点分十进制法，合法地址为0.0.0.0-255.255.255.255
bool isLegal(const char* pstr) {
    char* str = (char*) pstr;

    int validSegementSize = 0; //记录一共有多少个分段
    int oneSeg = 0; // 记录每个分段的数值
    int segLen = 0; // 记录分段是否有数值
    char c = *str;

    while (c != '\0') {
        //计算每个分段的数值
        if (c >= '0' && c <= '9') {
            oneSeg = oneSeg * 10 + (c - '0');
            //如果分段有数值，就置segLen为1
            segLen++;
        } else if (c == '.') {  //如果此字符为'.'，那么就判断之前的那个分段的值是否合法且是否存在值
            if (oneSeg >= 0 && oneSeg <= 255) {
                validSegementSize++;
            } else {
                return false;
            }
            oneSeg = 0; //重置分段值
            segLen = 0; //重置分段是否存在值
        } else { //如果出现0-9或'.'以外的字符都判断为非法
            return false;
        }
        str++;
        c = *str;
    }
    //判断最后一个分段的合法性
    if (oneSeg <= 255 && segLen >= 0) {
        validSegementSize++;
    }

    //判断是否一共有四个分段
    if (validSegementSize == 4) {
        return true;
    }
    return false;
}

int main() {
    char* str = "10.255.0.1";
    bool res = isLegal(str);
    std::cout << "res: " << res << std::endl;
    return 0;
}
```