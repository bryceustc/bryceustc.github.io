---
title: C++ bitset用法总结
date: 2020-05-04 16:02:03
tags: C++
categories: C++
---

### bitset

bitset是STL提供的用于记录01串的容器，也就是bitset的每个元素只能为0/1

bitset主要是用于存储二进制位，在需要大量二进制计算的项目中，直接使用bitset比利用数组或者其它方式要更为方便，位运算在存储状态、模拟行动、搜索很多方面有非常巨大的优势。
<!--more-->

### 头文件
```c++
#include<bitset>
```

### 构造函数
```c++
bitset<16> first;  //构造一个16位的bitset对象，初始值默认为0
bitset<16> second(177);  //构造一个16位的bitset对象，将177转换为二进制，拷贝到内存空间
bitset<16> secont_m(Oxff20);  //整型常量，可以直接十进制，也可以八进制，十六进制
bitset<16> third("1110011");  //将二进制字符串初始化到对象中
bitset<6> second_t(177);
bitset<6> third_t("1110011");

cout << first << endl;
cout << second << endl;
cout << third << endl;
cout << second_t << endl;
cout << third_t << endl;

for (int i = 0; i < 16; i++)
{
    cout << second[i];
}

//输出
/*
0000000000000000
0000000010110001
0000000001110011
110001
111001
1000110100000000
//由于bitset重载输出定向符号，为了便于查看，这里输出是逆向输出，长度为n的bitset，是按照n-1——>0的顺输出
*/
```

- 注意bitset声明后长度不可改变，和数组一样，bitset从0开始编号

- 如果bitset空间比二进制位数大，则在高位补0，(虽然上面输出是左边为0，但左边是高位，也即其索引分别是15~0)
- 在将整形常量转换为二进制过后，如果bitset位数不够，则自动放弃原数值高位的二进制位。

### bitset的操作
以下是bitset常用操作：
```c++
a.size() // a的位数
a.any()  //a中是否含1
a.none()  //a是否全为0
a.all() // a是否全为1
a.count()  //a中有几个1
a.[pos]  //访问第pos位
a.test(pos)  //第pos位是否为1
a.set()  //全部设为1
a.reset()  //全部清零
a.flip()  //全部取反
a.to_ulong()  //转成32位无符号整数
a.to_ullong() // 转换为64位无符号整数，如果超出范围则报错
a.to_string() // 转换为string
```
或者也可以对bitset使用位操作（返回一个bitset）：
```c++
a|b
a&b
a^b
~a
a<<1
a>>1
```

### bitset的原理
bitset的原理与高精度压位类似即：

将一个很长的01串按64位一组划分每组01串用一个64位无符号整数记录

bitset所有的操作都是基于对整数的位操作实现的。
所以bitset的效率非常高，可以看作O(N/64)

### bitset应用
可以方便利用bitset实现十进制到二进制的互相转换。

二进制转成十进制
```c++
#include<bits/stdc++.h>
using namespace std;
int main()
{
    bitset<16> a; // 16 bit 二进制数据，还有 bitset<32>
    cin >> a;
    cout << a.to_ulong() << endl;
    return 0;
}
```

十进制转成二进制
```c++
#include<bits/stdc++.h>
using namespace std;
int main()
{
    int b;
    cin >> b;
    bitset<16> a(b); // 16 bit 二进制数据，还有 bitset<32>
    cout << a<< endl;
    return 0;
}
```