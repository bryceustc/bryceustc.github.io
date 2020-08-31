---
title: 'C++ std::move用法'
date: 2020-08-28 09:40:33
tags: C++
categories: C++
---

std::move函数可以以非常简单的方式将左值引用转换为右值引用.

1. C++ 标准库使用比如vector::push_back 等这类函数时,会对参数的对象进行复制,连数据也会复制.这就会造成对象内存的额外创建, 本来原意是想把参数push_back进去就行了,通过std::move，可以避免不必要的拷贝操作。

2. std::move是将对象的状态或者所有权从一个对象转移到另一个对象，只是转移，没有内存的搬迁或者内存拷贝所以可以提高利用效率,改善性能.
<!--more-->
#### 用法

原lvalue值被moved from之后值被转移,所以为空字符串. 

```c++
#include <iostream>
#include <utility>
#include <vector>
#include <string>
int main()
{
    std::string str = "Hello";
    std::vector<std::string> v;
    //调用常规的拷贝构造函数，新建字符数组，拷贝数据
    v.push_back(str);
    std::cout << "After copy, str is \"" << str << "\"\n";
    //调用移动构造函数，掏空str，掏空后，最好不要使用str
    v.push_back(std::move(str));
    std::cout << "After move, str is \"" << str << "\"\n";
    std::cout << "The contents of the vector are \"" << v[0]
              << "\", \"" << v[1] << "\"\n";
}
```

输出
```
After copy, str is "Hello"
After move, str is ""
The contents of the vector are "Hello", "Hello" 
```

#### std::move 的函数原型定义

```c++
template <typename T>
typename remove_reference<T>::type&& move(T&& t)
{
	return static_cast<typename remove_reference<T>::type&&>(t);
```

#### 巨人的肩膀

https://blog.csdn.net/p942005405/article/details/84644069