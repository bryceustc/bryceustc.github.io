---
title: C++ 重载(overload)、重写(overrride)、重定义(redefine)总结
date: 2020-06-25 18:53:11
tags: C++
categories: C++
---
这三个概念很重要但是自己容易混淆，就整理记录一下。

### 一、重载（overload）
同一作用域内的函数名相同，但是它的参数表列个数或顺序，类型不同，就称之为重载。

（1）相同的范围（在同一个作用域中）；

（2）函数名字相同；

（3）参数不同；

（4）virtual 关键字可有可无；

（5）返回值可以不同；
<!--more-->
### 二、重写（override）

是指派生类重新定义基类的虚函数，特征是：

（1）不在同一个作用域（分别位于派生类与基类）；

（2）函数名字相同；

（3）参数相同；

（4）基类函数必须有 virtual 关键字，不能有 static 。

（5）返回值相同（或是协变），否则报错；<—-协变这个概念我也是第一次才知道…

（6）重写函数的访问修饰符可以不同。尽管 virtual 是 private 的，派生类中重写改写为 public,protected 也是可以的

### 三、重定义（也称为隐藏）
也叫做隐藏，子类重新定义父类中有相同名称的非虚函数 ( 参数列表可以不同 ) ，指派生类的函数屏蔽了与其同名的基类函数。可以理解成发生在继承中的重载。

（1）不在同一个作用域（分别位于派生类与基类）；

（2）函数名字相同；

（3）返回值可以不同；

（4）参数不同。此时，不论有无 virtual 关键字，基类的函数将被隐藏（注意别与重载以及重写混淆）。

（5）参数相同，但是基类函数没有 virtual关键字。此时，基类的函数被隐藏（注意别与重写混淆（有virtual 就是重写了））。

```c++
#include <iostream>
#include <string>

using namespace std;

class Base {
    public:
        void a(int x) {
            std::cout << "Base::a(int)" << std::endl;
        }

        // 函数重载
        void a(double x) {
            std::cout << "Base::a(double)" << std::endl;
        }

        virtual void b(int x) {
            std::cout << "Base::b(int)" << std::endl;
        }

        void c(int x) {
            std::cout << "Base::c(int)" << std::endl;
        }
};

class Derived : public Base {
    public:
        //函数的重定义 会隐藏父类同名方法 (参数不同)
        void a(char x) {
            std::cout << "Derived::a(char)" << std::endl;
        }
        //函数的重写， 覆盖父类的方法 override
        void b(int x) {
            std::cout << "Derived::b(int)" << std::endl;
        }
        //函数的重定义 会隐藏父类同名方法 (参数相同)
        void c(int x) {
            std::cout << "Derived::c(int)" << std::endl;
        }
};

int main() {
    Base B;
    Derived D;
    Base* pb = new Derived;

    // 重定义 父类的方法已经被隐藏 (参数不同的情形)
    D.a('c'); // Derived::a(char)
    B.a(1); // Base::a(int)

    // 函数重载 (参数类型不同)
    B.a(1.0);  // Base::a(double)
    
    // 基类中 a 不是虚函数没有发生重写，就依旧调用基类
    pb->a(1.0); // Base::a(double)

    B.b(1); // Base::b(int)
    D.b(1); // Derived::b(int)
    // b 是虚函数，在派生类中发生重写
    pb->b(1); // Derived::b(int)

    delete pb;

    return 0;
}
```

代码输出结果：
![](1.png)

通过这个实例可以看出：

1. Base类中的第二个函数a是对第一个的重载

2. Derived类中的函数b是对Base类中函数b的重写，即使用了虚函数特性。

3. Derived类中的函数a是对Base类中函数a的隐藏，即重定义了。

4. pb指针是一个指向Base类型的指针，但是它实际指向了一个Derived的空间，这里对pd调用函数的处理(多态性)取决于是否重写(虚函数特性)了函数，若没有，则依然调用基类。

5. 只有在通过基类指针或基类引用 间接指向派生类类型时多态性才会起作用。

6. 因为Base类的函数c没有定义为virtual虚函数，所以Derived类的函数c是对Base::c()的重定义。

### 参考：
1. cnblogs.com/tanky_woo/archive/2012/02/08/2343203.html