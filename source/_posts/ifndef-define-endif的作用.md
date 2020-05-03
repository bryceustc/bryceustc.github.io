---
title: '#ifndef/#define/#endif的作用'
date: 2020-05-02 17:21:22
tags: C++
categories: C++
---
头文件中的 #ifndef/#define/#endif 作用主要是防止该头文件被重复引用。

假设一个工程里面有4个文件，分别是a.cpp,b.h,c.h,d.h。
a.cpp的头部是：
```
#include "b.h "
#include "c.h "
```
<!--more-->
b.h和c.h的头部都是:
```
#include "d.h "
```
而d.h里面有class D的定义。

这样一来，
编译器编译a.cpp的时候，先根据#include "b.h "去编译b.h这个问题，再根据b.h里面的#include "d.h "，去编译d.h的这个文件，这样就把d.h里面的class D编译了；
然后再根据a.cpp的第二句#include "c.h "，去编译c.h，最终还是会找到的d.h里面的class D，但是class D之前已经编译过了，所以就会报重定义错误。

加上#ifndef/#define/#endif，就可以防止这种重定义错误。