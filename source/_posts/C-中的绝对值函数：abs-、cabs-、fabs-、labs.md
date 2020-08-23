---
title: C++ 中的绝对值函数：abs()、cabs()、fabs()、labs()
date: 2020-08-22 13:30:32
tags: C++
categories: C++
---
不同类型的数据使用不同类型的绝对值函数：

整型：

```c++
int abs(int i)  //返回整型参数i的绝对值 
```

双精度浮点型：
```c++
double fabs(double x)  //返回双精度参数x的绝对值    
```

长整型：
```c++
long labs(long n)  //返回长整型参数n的绝对值 
```

复数：
```c++
double cabs(struct complex znum)  //返回复数znum的绝对值  
```