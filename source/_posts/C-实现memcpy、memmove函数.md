---
title: C++ 实现memcpy、memmove函数
date: 2024-05-13 23:05:03
tags: C++
categories: C++
---

记录有道面试题实现memmove函数，C语言语法有点生疏了，Practice Makes Perfect.

memcpy与memmove的目的都是将N个字节的源内存地址的内容拷贝到目标内存地址中。但当源内存和目标内存存在重叠时，**memcpy不保证拷贝结果的正确，而memmove能正确地实施拷贝**，但这也增加了一点点开销。

memcpy的实现：

- 当源内存的首地址等于目标内存的首地址时，不进行任何拷贝
- 当源内存的首地址不等于目标内存的首地址时，实行正向拷贝

memmove的实现：

- 当源内存的首地址等于目标内存的首地址时，不进行任何拷贝
- 当源内存的首地址大于目标内存的首地址时，实行正向拷贝
- 当源内存的首地址小于目标内存的首地址时，实行反向拷贝
<!--more-->

具体代码实现：

```c++
#include <string>
#include <iostream>

using namespace std;


void* MyMemcpy (void * dst, const void * src, size_t count) {

    // 判断异常条件
    if (count == 0) return dst;
    if (dst == nullptr || src == nullptr)  return nullptr;

    char* p_dst = (char*) dst;
    char* p_src = (char*) src;

    while(count--) {
        *p_dst = *p_src;
        p_dst++;
        p_src++;
    }
    return dst;

}

void* MyMemmove (void * dst, const void * src, size_t count) {

    // 判断异常条件
    if (count == 0) return dst;
    if (dst == nullptr || src == nullptr)  return nullptr;

    char* p_dst = (char*) dst;
    char* p_src = (char*) src;

    // 如果不发生重叠，正向复制
    if (p_dst <= p_src || p_src + count <= p_dst) {
        while(count--) {
            *p_dst = *p_src;
            p_dst++;
            p_src++;
        }
    } else {
        // 反向复制
        p_dst = p_dst + count - 1;
        p_src = p_src + count - 1;
        while (count--) {
            *p_src = *p_dst;
            p_src--;
            p_dst--;
        }
    }
    return dst;
}

int main() {
    char buf[] = "abcdefg";
    char str[] = "abcdefg";

    MyMemcpy(str + 2, str, 4);
    cout << str << std::endl; // abababg
    MyMemmove(buf, buf + 2, 2); 
    cout << buf << std::endl; // cdcdefg
    return 0;
}
```