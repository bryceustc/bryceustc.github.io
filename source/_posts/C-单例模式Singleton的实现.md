---
title: C++单例模式Singleton的实现
date: 2020-04-01 20:27:44
tags: C++
categories: C++
---
### 问题情景

在windows下面打开任务管理器，我们会发现，你只能打开一个任务管理器。如果在已打开一个任务管理器的情况下，再次打开任务管理器，系统会调出已经打开的窗口，而不是重新打开新的窗口。在系统中，之多只能存在一个任务管理器的实例。按照一般的思路，我们需要定义一个TaskManager的类（如下）
```c++
class TaskManager
{
public:
  TaskManager(){}
}
```
<!--more-->

但这存在问题，只要我们调用一次初始化函数，系统就会生成一个TaskManager的实例，不能保证系统中之多只存在一个实例的要求。于是我们需要采用设计模式中的单例模式（Singleton）来实现。

### 单例模式

定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点。

单例模式的两个功能：仅有一个实例和全局访问点。我们通过Singleton的类图来看一下这个模式：

![](1.png)

在Singleton中，类的初始化函数被设为了private，这就意味着Client是不能通过调用类的初始化函数来生成一个实例，防止Client多次调用初始化函数生成多个实例出来。如果要访问Singleton，需要通过GetInstance方法，这是一个静态的方法，主要负责创建自己的实例，返回实例的指针给Client。

```c++
#include <iostream>
using namespace std;
 
class Singleton
{
    public:
        static Singleton* GetInstance()
        {
            if (p_instance == NULL)//第一次调用要判断指针是否为空
            {
                p_instance = new Singleton();
            }
            return p_instance;
        }
    private:
        Singleton(){}; //构造函数设计成私有的
        static Singleton *p_instance; //用类的指针指向唯一的实例
};
Singleton* Singleton::p_instance = NULL;
//静态数据成员是静态存储的，必须对他进行初始化

int main(int argc, char*argv[])
{
    Singleton *object = Singleton::GetInstance();
    return 0;
}
```

Singleton类封装了它的唯一实例，这样它可以严格地控制客户怎样访问它以及何时访问它。简单的说就是对唯一实例的受控访问。一般的，Singleton不会定义析构函数，由于实例分配的是全局地址，在程序运行结束，被系统收回所有分配空间时，Singleton才会被销毁，如果Singleton成员数据太多的话，可能占用了大量的内存空间而不能被释放，这需要我们注意。

### Singleton与全局变量的区别

Singleton与全局变量都有全局访问的功能，但两者之间存在一些差别，全局变量在使用的过程中会存在一些问题：

- 变量名冲突：必须小心维护变量名，每遇到一个全局变量，都必须仔细分辨该变量属于哪个模块、哪个程序。
- 耦合度难题：使用全局变量增加了函数与模块间的耦合程度，不易维护。
- 单个实体问题：全局变量不能防止一个类生成多个实例出来。
- 多线程访问：并发访问时需要使用同步机制，小心保护全局变量，避免冲突。

看起来，使用Singleton可以避免上面的这些问题，但Singleton自身也有一些弊端。

### Singleton的优缺点

Singleton的优点：
- 跨平台：使用合适的中间件可以把Singleton扩展为跨多个计算机工作。
- 适用于任何类：只要将初始化函数设为私有，并增加相应的静态函数和变量，就能把类变成Singleton。
- 延迟性：如果Singleton从未使用，就不会创建（仅指懒汉模式）

Singleton的缺点：

- 效率问题：由于if语句的存在，调用方法的效率收到影响。
- 不变重用：在C++下需要定义模板才能实现Singleton的重用。

### Singleton的懒汉模式与饿汉模式

针对Singleton初始化函数的调用策略，有两种选择，分别是懒汉模式和饿汉模式。

#### 懒汉模式

使用懒汉模式时，Singleton在程序第一次调用的时候才会初始化自己，代码如下。使用该模式时，由于if语句的存在，会影响调用的效率。而且，在多线程环境下使用时，为了保证只能初始化一个实例，需要用锁来保证线程安全性，防止同时多个线程进入if语句中。如果遇到处理大量数据时，锁会成为整个性能的瓶颈。一般懒汉模式适用于程序一部分中需要使用Singleton，且在实例化后没有大量频繁访问或线程访问的情况。
```c++
#include <iostream>
#include <thread> 
#include <mutex>
using namespace std;
std::mutex mtx;
 
class Singleton
{
    public:
        static Singleton* GetInstance()
        {
            if (p_instance == NULL)
            {
                mtx.lock();
                if (p_instance == NULL)
                {
                    p_instance = new Singleton();
                }
                mtx.unlock();
            }
            return p_instance;
        }
    private:
        Singleton(){}; //构造函数设计成私有的
        static Singleton *p_instance; //用类的指针指向唯一的实例
};
Singleton* Singleton::p_instance = NULL;
//静态数据成员是静态存储的，必须对他进行初始化

int main()
{
    Singleton *object = Singleton::GetInstance();
    return 0;
}
```

    PS:
    GCC在Linux下的std::mutex工作良好。 然而，在Windows上很遗憾，
    _GLIBCXX_HAS_GTHREADS没有在Windows上定义。mutex运行时支持是不存在的。

#### 饿汉模式
使用饿汉模式时，Singleton在程序一开始就将自己实例化，之后的GetInstance方法仅返回实例的指针即可，这样就解决了上述提到的if语句影响效率的问题。代码如下：
```c++
class Singleton
{
    private:
        Singleton* p_instance = new Singleton();//在此直接实例化
        Singleton(){};
    public:
        static Singleton* GetInstance()
        {
            return p_instance;
        }
};
```

饿汉模式适用于Singleton在程序运行过程中一直被频繁调用，这样由于预先加载了实例，访问实例时没有if语句，效率更高。但要注意到，如果Singleton的成员比较庞大、复杂，实例化Singleton会花一些时间，且这个实例一直占用着大量内存，在使用时要注意这部分的开销。使用饿汉模式用于多线程编程的话，由于线程访问之前，实例已存在，就不需要像懒汉模式中加入锁，因此饿汉模式保证了多线程安全。饿汉模式比较适用于程序整个运行过程中都需要访问、会被频繁访问或者需要被多线程访问的情况。

    PS:
    类名加*，有两种作用情况：
    1. 用于定义时，或用于函数参数，返回值时，表示对应变量值为类指针类型；
    2. 用于变量前，以(CLASS_NAME *)var_name，形式出现时，表示将变量或
    常量var_name强制转换为类指针类型。
