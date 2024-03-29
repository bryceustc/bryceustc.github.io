---
title: 剑指offer（一）：赋值运算符函数
date: 2023-09-16 14:25:09
tags: 剑指Offer
categories: 剑指Offer
mathjax: true
---

# 题目描述:

如下为类型CMyString的声明。请为该类型添加赋值运算符函数。
```c++
class CMyString
{
public:
     // 构造函数
     CMyString(char*pData = nullptr);
     // 拷贝构造函数 (用来初始化对象)；
     CMyString(const CMyString & str);
     // 析构函数
     ~CMyString(void);
private:
     char* m_pData;
};
```
<!--more-->
# 本题考点：
  
  1). C++基础语法的理解，如运算符函数、常量引用等。
  
  2). 对内存泄漏的理解
  
  3). 代码异常安全性的理解
  
# 解题思路:

**考虑因素：**

  1). 返回值的类型应该声明为该类型的引用，并在函数结束前返回实例自身的引用(*this)，因为只有返回一个引用时才可以允许被连续赋值；

  2). 把传入的参数类型声明为常量引用(注意：拷贝构造函数一定要为**常量引用**，如果不是引用而是实例，则会从形参到实参再回调用一次拷贝构造函数，造成**无限调用**)；

  3). 注意释放实例自身已有的内存；

  4). 判断传入的参数和当前的实例(\*this)是不是同一个实例则不需要进行赋值操作，直接返回。如果事先不判断就赋值，那么在释放实例自身内存时，之后再赋值就会找不到赋值的内容。

**进一步思考**

  1). 其实就是一个构造函数，不过形参是该类型本身。
  
  2). 形参为const的引用，因为拷贝不影响原变量。如果不使用引用会无限循环调用构造函数。
  
  3). 在给新建变量赋值时会引用。而赋值运算符用在给已存在变量赋值的情况。

# 代码

[C++](https://github.com/bryceustc/CodingInterviews/blob/master/CMyString/CMyString.cpp)

## 方法一：
```c++
// CMyString.h文件中

#ifndef INC_1_CMYSTRING_H
#define INC_1_CMYSTRING_H

#include <string.h>
#include <iostream>

class CMyString {
public:
    CMyString(char *pData = nullptr);
    CMyString(const CMyString &);
    ~CMyString();
    CMyString &operator = (const CMyString &str);
    char *getData();
private:
    char *m_pData;
};

CMyString::CMyString(char *pData){
    m_pData = new char[strlen(pData) + 1];
    strcpy(m_pData, pData);
}

CMyString::~CMyString() {
    if (m_pData) {
        delete[] m_pData;
        m_pData = nullptr;
    }
}

//拷贝构造函数
CMyString::CMyString(const CMyString &str) {
    std::cout << "这是拷贝构造函数！" << std::endl;
    m_pData = new char[strlen(str.m_pData) + 1];
    strcpy(m_pData, str.m_pData);
}

//赋值运算符函数
CMyString &CMyString::operator=(const CMyString &str) {
    std::cout << "这是拷贝赋值运算符！" << std::endl;
    if (this == &str) {
        //检查自赋值的情况
        return *this;
    }

    //释放原本的内存
    if (m_pData) {
        delete[] m_pData;
        m_pData = nullptr;
    }

    // 分配新的内存资源
    m_pData = new char[strlen(str.m_pData) + 1];
    strcpy(m_pData, str.m_pData);

    // 返回本对象的引用
    return *this;
}

char *CMyString::getData() {
    return m_pData;
}

#endif //INC_1_CMYSTRING_H


```

```c++
// CMyString.cpp文件中

#include "CMyString.h"
#include <iostream>

using namespace std;

int main()
{
    char *temp = "Hello World";
    CMyString myStr(temp);

    cout << "myStr: " << myStr.getData() << endl;

    CMyString test = myStr;

    cout << "test: " << test.getData() << endl;

    char *temp2 = "show the difference.";
    CMyString myStr2(temp2);
    cout << "myStr2: " << myStr2.getData() << endl;

    myStr2 = test;
    cout << "myStr2 after operator \"=\": " << myStr2.getData() << endl;

    return 0;
}
```


```c++
//main.cpp中的测试程序

#include "CMyString.h"
#include <iostream>

using namespace std;

int main()
{
    char *temp = "Hello World";
    CMyString myStr(temp);

    cout << "myStr: " << myStr.getData() << endl;

    CMyString test = myStr; // 类似 CMyString test(myStr)这种的浅拷贝

    cout << "test: " << test.getData() << endl;

    char *temp2 = "show the difference.";
    CMyString myStr2(temp2);
    cout << "myStr2: " << myStr2.getData() << endl;

    myStr2 = test;
    cout << "myStr2 after operator \"=\": " << myStr2.getData() << endl;

    return 0;
}
```


```
输出结果：
myStr: Hello World
这是拷贝构造函数！
test: Hello World
myStr2: show the difference.
这是拷贝赋值运算符！
myStr2 after operator "=": Hello World
```

## 更完善版本：考虑new分配空间是否足够，内存不足时，直接new会抛出异常，
```c++
CMyString &CMyString::operator=(const CMyString &str) {
    if (this != &str) {
        //调用拷贝构造函数，构造临时变量
        CMyString strTmp(str);
        // 交换当前对象与临时对象的数据。
        char *pTmp = strTmp.m_pData;
        strTmp.m_pData = m_pData;
        m_pData = pTmp;
    }

    return *this;
}
```

# 参考：
- [赋值运算符](https://www.runoob.com/cplusplus/assignment-operators-overloading.html):就像其他运算符一样，可以重载赋值运算符（ = ），用于创建一个对象，比如拷贝构造函数。

- [this指针](https://www.runoob.com/cplusplus/cpp-this-pointer.html):在 C++ 中，每一个对象都能通过 this 指针来访问自己的地址。this 指针是所有成员函数的隐含参数。因此，在成员函数内部，它可以用来指向调用对象。

- [构造函数](https://www.runoob.com/cplusplus/cpp-constructor-destructor.html):类的构造函数是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于为某些成员变量设置初始值。

- [析构函数](https://www.runoob.com/cplusplus/cpp-constructor-destructor.html):类的析构函数是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。**析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源**。
