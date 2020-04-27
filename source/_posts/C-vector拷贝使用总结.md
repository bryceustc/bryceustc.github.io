---
title: C++ vector拷贝使用总结
date: 2020-04-02 09:51:23
tags: C++
categories: C++
---
一些vector拷贝使用的细节，挖过填过的坑，记录一下。

主要有以下几种方法
### 初始化构造时拷贝（深复制）

这种拷贝，相当于复制了一份数据，array中的数据不变。 如下
```c++
vector<int> array{3,5,2,6,4};
vector<int> outArray(array);
```
<!--more-->
### 利用拷贝赋值操作符（深复制）

```c++
vector<int> array{3,5,2,6,4};
vector<int> outArray;
outArray = array;
```

### 利用assign()函数（清空并深复制）

一样的复制了一份数据，array中的数据不变。

```c++
vector<int> array{3, 5, 2, 6, 4};
vector<int> outArray{ 1,2,3 };
outArray.assign(array.begin(),array.end());//清空原数据，赋予新数据={3,5,2,6,4}
outArray.assign(5, 0);//5个0，清空原数据={0,0,0,0,0,}
```

### 利用swap()函数（交换两个vector）

会清空原vector数组

```c++
vector<int> array{3,5,2,6,4};
vector<int> outArray;//设为空
outArray.swap(array);//清空array数组
```
```c++
vector<int> array{3, 5, 2, 6, 4};
vector<int> outArray{ 1,2,3 };
outArray.swap(array);//outArray={3,5,2,6,4}；array={1,2,3}
```