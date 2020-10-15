---
title: extern关键字总结
date: 2020-09-18 13:50:01
tags: C++
categories: C++
mathjax: true
---
### extern

**extern**是一种“**外部声明**”的关键字，字面意思就是**在此处声明**某种变量或函数，**在外部定义**。

extern关键字的主要作用是扩大变量/函数的作用域，使得其它源文件和头文件可以复用同样的变量/函数，也起到类似“分块储存”的作用，划分代码。如图所示，在一个头文件里做了外部声明，就能把变量的定义部分和函数体的实现部分转移到其它地方了，也就是extern可以实现多文件**共享**同一个变量、const常量、函数。
<!--more-->
例子1：

```C++
//file1.cpp
#include<iostream>
using namespace std;

extern int count;

int main(){

	cout<<count<<endl;
	return 0;

}


//file2.cpp

int count=3;
```

输出：将两个文件一起编译，则输出会是3

因为file1使用了在file2中定义的全局变量count，全局变量也叫外部变量，具有外部链接性，意思就是可以被外部文件引用。

**注意：**当使用extern声明变量时，要求被声明的变量只能在一个文件中被定义，比如再有个file3里面也定义了一个名为count的全局变量，然后跟file1、file2一起编译，那编译器就会报错，因为不知道要引用哪个count，这也被称为单独定义规则。

总而言之，在多文件程序中，可以在一个文件（且只能在一个文件）中定义一个外部变量。使用该变量的其他文件必须使用extern来声明他。(摘自C++ Primer Plus)

例子2：

```C++
//file1.cpp
#include<iostream>
using namespace std;

void f(int x){

	if(x>3) return;
	extern int count;
	count++;
	cout<<count<<endl;
	f(x+1);
}

int main(){
	f(1);
	return 0;
}

//file2.cpp

int count=1;
...
输出：
file1和file2一起编译，运行后的输出是：
2
3
4
```

因为file1使用extern声明的count实际上就是file2中的count（同一个存储地址），file1只是声明要引用file2中的count，而不是重新定义一个count，所以即使递归调用函数也不会影响count保留上一次的值。这一点跟用static定义一个内部变量很像，不同的是static定义的内部变量只能初始化一次，而extern声明的变量不能初始化。

例子3：

```C++
//file1.cpp
#include<iostream>
using namespace std;

extern const int x;

int main(){
    cout<<x<<endl;
    return 0;
}

//file2.cpp
extern const int x=5;
...
file1和file2一起编译，运行后的输出是5
```

显然file1通过extern声明引用了在file2中的const常量x。

**注意：**file1和file2都要加extern，才能实现多个文件共享一个const常量

因为const声明的常量它的链接性是内部的，默认是不能被其他文件用extern引用的。也就是说例1中说的单定义规则对它并不适用，所以即使在多个文件中定义同名的const常量，也不会有问题。

可以使用extern来覆盖其默认的内部链接性，就像file2中做得那样，并且只能在一个文件中被初始化。

### extern "C"

是用于C++链接在C语言模块中定义的函数。

#### 1. C++与C编译的区别

C++虽然兼容C，但C++文件中函数编译后生成的符号与C语言生成的不同。因为C++支持函数重载，C++函数编译后生成的符号带有函数参数类型的信息，而C则没有。

例如`int add(int a, int b)`函数经过C++编译器生成.o文件后，`add`会变成形如`add_int_int`之类的, 而C的话则会是形如`_add`, 就是说：相同的函数，在C和C++中，编译后生成的符号不同。

这就导致一个问题：如果C++中使用C语言实现的函数，在编译链接的时候，会出错，提示找不到对应的符号。此时`extern "C"`就起作用了：告诉链接器去寻找`_add`这类的C语言符号，而不是经过C++修饰的符号。

### 2. C++调用C函数

```C++
//add.h
#ifndef ADD_H
#define ADD_H
int add(int x,int y);
#endif

//add.c
#include "add.h"
int add(int x,int y) {
    return x+y;
}

//add.cpp
#include <iostream>
extern "C" {
    #include "add.h"
}
using namespace std;
int main() {
    add(2,3);
    return 0;
}
```

### 3. C调用C++

```c++
// add.h
#ifndef ADD_H
#define ADD_H
extern "C" {
    int add(int x,int y);
}
#endif

// add.cpp
#include "add.h"

int add(int x,int y) {
    return x+y;
}

// add.c
extern int add(int x,int y);
int main() {
    add(2,3);
    return 0;
}
```

综上，总结出使用方法，在C语言的头文件中，对其外部函数只能指定为extern类型，C语言中不支持extern "C"声明，在.c文件中包含了extern "C"时会出现编译语法错误。所以使用extern "C"全部都放在于cpp程序相关文件或其头文件中。

