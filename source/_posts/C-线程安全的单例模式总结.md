---
title: C++线程安全的单例模式总结
date: 2020-07-19 17:25:07
tags: 设计模式
categories: 设计模式
---
### 什么是线程安全？
在拥有共享数据的多条线程并行执行的程序中，线程安全的代码会通过同步机制保证各个线程都可以正常且正确的执行，不会出现数据污染等意外情况。

### 如何保证线程安全？
1. 给共享的资源加把锁，保证每个资源变量每时每刻至多被一个线程占用。
2. 让线程也拥有资源，不用去共享进程中的资源。如： 使用threadlocal可以为每个线程的维护一个私有的本地变量。

### 什么是单例模式？
单例模式指在整个系统生命周期里，保证一个类只能产生一个实例，确保该类的**唯一性**。

#### 单例模式分类

单例模式可以分为**懒汉式**和**饿汉式**，两者之间的区别在于创建实例的时间不同：

- 懒汉式：指系统运行中，实例并不存在，只有当需要使用该实例时，才会去创建并使用实例。（这种方式要考虑线程安全）
- 饿汉式：指系统一运行，就初始化创建实例，当需要时，直接调用即可。（本身就线程安全，没有多线程的问题）
<!--more-->
#### 单例类的特点
- 构造函数和析构函数为private类型，目的禁止外部构造和析构
- 拷贝构造和赋值构造函数为private类型，目的是禁止外部拷贝和赋值，确保实例的唯一性
- 类里有个获取实例的静态函数，可以全局访问

### 01 普通懒汉式单例 （ 线程不安全 ）
```c++
///////////////////  普通懒汉式实现 -- 线程不安全 //////////////////
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

using namespace std;

class SingleInstance
{

public:
    // 获取单例对象
    static SingleInstance *GetInstance();

    // 释放单例，进程退出时调用
    static void deleteInstance();
    
    // 打印单例地址
    void Print();

private:
    // 将其构造和析构成为私有的, 禁止外部构造和析构
    SingleInstance();
    ~SingleInstance();

    // 将其拷贝构造和赋值构造成为私有函数, 禁止外部拷贝和赋值
    SingleInstance(const SingleInstance &signal);
    const SingleInstance &operator=(const SingleInstance &signal);

private:
    // 唯一单例对象指针
    static SingleInstance *m_SingleInstance;
};

//初始化静态成员变量
SingleInstance *SingleInstance::m_SingleInstance = NULL;

SingleInstance* SingleInstance::GetInstance()
{

    if (m_SingleInstance == NULL)
    {
        m_SingleInstance = new SingleInstance();  // 没有加锁是线程不安全的，当线程并发时会创建多个实例
    }

    return m_SingleInstance;
}

void SingleInstance::deleteInstance()
{
    if (m_SingleInstance)
    {
        delete m_SingleInstance;
        m_SingleInstance = NULL;
    }
}

void SingleInstance::Print()
{
    std::cout << "实例内存地址:" << this << std::endl;
}

SingleInstance::SingleInstance()
{
    std::cout << "构造函数" << std::endl;
}

SingleInstance::~SingleInstance()
{
    std::cout << "析构函数" << std::endl;
}
///////////////////  普通懒汉式实现 -- 线程不安全  //////////////////

// 线程函数
void Print(int id)
{
    std::cout << "线程 ID:[" << id << "]" << std::endl;

    // 打印实例地址
    SingleInstance::GetInstance()->Print();
}

#define NUM_THREADS 2 // 线程个数

int main()
{
    vector<thread> thread_group;

    for (int i = 0; i < NUM_THREADS; i++)
    {
        std::cout << "创建线程:[" << i << "]" << std::endl;
        thread_group.push_back(thread(Print,i));
    }
    for (int i = 0; i < NUM_THREADS; i++) {
        thread_group[i].join();
    }
    // 手动释放单实例的资源
    SingleInstance::deleteInstance();
    
    return 0;
}
```

#### 普通懒汉式单例运行结果：
从运行结果可知，单例构造函数创建了两个，内存地址分别为0x102c04570和0x102b05670，所以普通懒汉式单例只适合单进程不适合多线程，因为是线程不安全的。
![](2.png)

### 02 加锁的懒汉式单例 （ 线程安全 ）
```c++
///////////////////  加锁的懒汉式实现  //////////////////
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

using namespace std;

class SingleInstance
{

public:
    // 获取单例对象
    static SingleInstance *GetInstance();

    // 释放单例，进程退出时调用
    static void deleteInstance();
    
    // 打印单例地址
    void Print();

private:
    // 将其构造和析构成为私有的, 禁止外部构造和析构
    SingleInstance();
    ~SingleInstance();

    // 将其拷贝构造和赋值构造成为私有函数, 禁止外部拷贝和赋值
    SingleInstance(const SingleInstance &signal);
    const SingleInstance &operator=(const SingleInstance &signal);

private:
    // 唯一单例对象指针
    static SingleInstance *m_SingleInstance;
    static std::mutex m_Mutex;
};

//初始化静态成员变量
SingleInstance *SingleInstance::m_SingleInstance = NULL;
std::mutex SingleInstance::m_Mutex;

SingleInstance* SingleInstance::GetInstance()
{
    //  这里使用了两个 if判断语句的技术称为双检锁；好处是，只有判断指针为空的时候才加锁，
    //  避免每次调用 GetInstance的方法都加锁，锁的开销毕竟还是有点大的。
    if (m_SingleInstance == NULL)
    {
        m_Mutex.lock(); // 加锁
        if (m_SingleInstance == NULL)
        {
            m_SingleInstance = new SingleInstance();
        }
        m_Mutex.unlock(); // 解锁
    }

    return m_SingleInstance;
}

void SingleInstance::deleteInstance()
{
    
    if (m_SingleInstance)
    {
        delete m_SingleInstance;
        m_SingleInstance = NULL;
    }
}

void SingleInstance::Print()
{
    std::cout << "实例内存地址:" << this << std::endl;
}

SingleInstance::SingleInstance()
{
    std::cout << "构造函数" << std::endl;
}

SingleInstance::~SingleInstance()
{
    std::cout << "析构函数" << std::endl;
}
///////////////////  加锁的懒汉式实现  //////////////////
```
#### 加锁的懒汉式单例的运行结果：
从运行结果可知，只创建了一个实例，内存地址是0x10050d300，所以加了互斥锁的普通懒汉式是线程安全的
![](3.png)
### 03 内部静态变量的懒汉单例（C++11 线程安全）
```c++
///////////////////  内部静态变量的懒汉实现  //////////////////
class SingleInstance
{

public:
    // 获取单实例对象
    static SingleInstance &GetInstance();

    // 打印实例地址
    void Print();

private:
    // 禁止外部构造
    SingleInstance();

    // 禁止外部析构
    ~SingleInstance();

    // 禁止外部复制构造
    SingleInstance(const SingleInstance &signal);

    // 禁止外部赋值操作
    const SingleInstance &operator=(const SingleInstance &signal);
};

SingleInstance &SingleInstance::GetInstance()
{
    // 局部静态特性的方式实现单实例
    static SingleInstance signal;
    return signal;
}

void SingleInstance::Print()
{
    std::cout << "实例内存地址:" << this << std::endl;
}

SingleInstance::SingleInstance()
{
    std::cout << "构造函数" << std::endl;
}

SingleInstance::~SingleInstance()
{
    std::cout << "析构函数" << std::endl;
}
///////////////////  内部静态变量的懒汉实现  //////////////////
```
#### 内部静态变量的懒汉单例的运行结果：
使用了C++11的特性，在C++11内部静态变量的方式里是线程安全的，只创建了一次实例，内存地址是0x100006168，这个方式非常推荐，实现的代码最少！
![](4.png)

### 04 饿汉式单例 （本身就线程安全）
```c++
///////////////////  普通懒汉式实现 -- 线程不安全 //////////////////
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

using namespace std;

////////////////////////// 饿汉实现 /////////////////////
class SingleInstance
{
public:
    // 获取单实例
    static SingleInstance* GetInstance();

    // 释放单实例，进程退出时调用
    static void deleteInstance();
    
    // 打印实例地址
    void Print();

private:
    // 将其构造和析构成为私有的, 禁止外部构造和析构
    SingleInstance();
    ~SingleInstance();

    // 将其拷贝构造和赋值构造成为私有函数, 禁止外部拷贝和赋值
    SingleInstance(const SingleInstance &signal);
    const SingleInstance &operator=(const SingleInstance &signal);

private:
    // 唯一单实例对象指针
    static SingleInstance *g_pSingleInstance;
};

// 代码一运行就初始化创建实例 ，本身就线程安全
SingleInstance* SingleInstance::g_pSingleInstance = new SingleInstance();

SingleInstance* SingleInstance::GetInstance()
{
    return g_pSingleInstance;
}

void SingleInstance::deleteInstance()
{
    if (g_pSingleInstance)
    {
        delete g_pSingleInstance;
        g_pSingleInstance = NULL;
    }
}

void SingleInstance::Print()
{
    std::cout << "实例内存地址:" << this << std::endl;
}

SingleInstance::SingleInstance()
{
    std::cout << "构造函数" << std::endl;
}

SingleInstance::~SingleInstance()
{
    std::cout << "析构函数" << std::endl;
}
////////////////////////// 饿汉实现 /////////////////////

// 线程函数
void Print(int id)
{
    std::cout << "线程 ID:[" << id << "]" << std::endl;

    // 打印实例地址
    SingleInstance::GetInstance()->Print();
}

#define NUM_THREADS 2 // 线程个数

int main()
{
    vector<thread> thread_group;

    for (int i = 0; i < NUM_THREADS; i++)
    {
        std::cout << "创建线程:[" << i << "]" << std::endl;
        thread_group.push_back(thread(Print,i));
    }
    for (int i = 0; i < NUM_THREADS; i++) {
        thread_group[i].join();
    }
    
    return 0;
}
```
#### 饿汉式单例的运行结果：
从运行结果可知，饿汉式在程序一开始就构造函数初始化了，所以本身就线程安全的。代码一运行就初始化创建单实例。
![](5.png)

### 特点与选择
- 懒汉式是以时间换空间，适应于访问量较小时；推荐使用内部静态变量的懒汉单例，代码量少
- 饿汉式是以空间换时间，适应于访问量较大时，或者线程比较多的的情况