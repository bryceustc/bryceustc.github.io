---
title: 剑指offer（二）：实现Singleton模式
date: 2023-09-16 20:24:39
tags: 剑指Offer
categories: 剑指Offer
mathjax: true
---
# 题目描述：
设计一个类，我们只能生成该类的一个实例
# 本题考点：
  
  1). 考察对单例的(Singleton)模式的理解。
  
  2). 基础语法的理解，如静态构造函数
  
  3). 多线程编程的理解
  
<!--more-->
# 解题思路：

书中本节的代码是采用C#编写的，这里用C++实现。

# 代码：

## 方法一：单线程解法

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

## 方法二：多线程+加锁
```c++
// 懒汉模式：Singleton在程序第一次调用的时候才会初始化自己
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
            //  这里使用了两个 if判断语句的技术称为双检锁；好处是，只有判断指针为空的时候才加锁，
            //  避免每次调用 GetInstance的方法都加锁，锁的开销毕竟还是有点大的。
            if (p_instance == NULL)
            {
                mtx.lock(); // 使用C++11中的多线程库
                if (p_instance == NULL)
                {
                    p_instance = new Singleton();
                }
                mtx.unlock();
            }
            return p_instance;
        }
    private:
        static Singleton *p_instance; //用类的指针指向唯一的实例
        // 将其构造和析构成为私有的, 禁止外部构造和析构
        Singleton();
        ~Singleton();
        // 将其拷贝构造和赋值构造成为私有函数, 禁止外部拷贝和赋值
        Singleton(const Singleton &signal);
        const Singleton &operator=(const Singleton &signal);
};
Singleton* Singleton::p_instance = NULL;
//静态数据成员是静态存储的，必须对他进行初始化

// 饿汉模式
class Singleton
{
    public:
        static Singleton* GetInstance()
        {
            return p_instance;
        }
    private:
        static Singleton *p_instance; //用类的指针指向唯一的实例
        // 将其构造和析构成为私有的, 禁止外部构造和析构
        Singleton();
        ~Singleton();
        // 将其拷贝构造和赋值构造成为私有函数, 禁止外部拷贝和赋值
        Singleton(const Singleton &signal);
        const Singleton &operator=(const Singleton &signal);
};
// 代码一运行就初始化创建实例 ，本身就线程安全
Singleton* Singleton::p_instance = new Singleton();

int main()
{
    Singleton *object = Singleton::GetInstance();
    return 0;
}
```
## 方法三：const + 实例
```c++
#include <iostream>
    #include <thread>
    #include <vector>
    using namespace std;
    
    class Singleton
    {
    private:
        Singleton(){}
        static const Singleton* m_pInstance;
    public:
        static Singleton* getInstance(){
            return const_cast<Singleton*>(m_pInstance); // 去掉“const”特性
            // 注意！若该函数的返回值改为const static型，则此处不必进行const_cast静态转换
            // 所以该函数可以改为：
            /*
            const static Singleton* getInstance(){
                return m_pInstance;
            }
            */
        }
    
        static void destroyInstance(){
            if(m_pInstance != NULL){
                delete m_pInstance;
                m_pInstance = NULL;
            }
        }
    };
    const Singleton* Singleton::m_pInstance = new Singleton(); // 利用const只能定义一次，不能再次修改的特性，static继续保持类内只有一个实例
    
    void print_singleton_instance(){
        Singleton *singletonObj = Singleton::getInstance();
        cout << singletonObj << endl;
    }
    
    // 多个进程获得单例
    void Test1(){
        // 预期结果，打印出相同的地址，之间可能缺失换行符，也属正常现象
        vector<thread> threads;
        for(int i = 0; i < 10; ++i){
            threads.push_back(thread(print_singleton_instance));
        }
        for(auto& thr : threads){
            thr.join();
        }
    }
    
    int main(){
        Test1();
        Singleton::destroyInstance();
        return 0;
    }
    /*因为静态初始化在程序开始时，也就是进入主函数之前，由主线程以单线程方式完成了初始化，所以静态初始化实例保证了线程安全性。在性能要求比较高时，就可以使用这种方式，从而避免频繁的加锁和解锁造成的资源浪费。由于上述三种实现，都要考虑到实例的销毁，关于实例的销毁，待会在分析。*
```

## 方法四：在get函数中创建并返回static临时实例的引用
```c++
#include <iostream>
#include <thread>
#include <vector>
using namespace std;

class Singleton
{
private:
    Singleton(){}

public:
    static Singleton* getInstance(){
        static Singleton m_pInstance; // 注意，声明在该函数内
        return &m_pInstance;
    }
};

void print_singleton_instance(){
    Singleton *singletonObj = Singleton::getInstance();
    cout << singletonObj << endl;
}

// 多个进程获得单例
void Test1(){
    // 预期结果，打印出相同的地址，之间可能缺失换行符，也属正常现象
    vector<thread> threads;
    for(int i = 0; i < 10; ++i){
        threads.push_back(thread(print_singleton_instance));
    }

    for(auto& thr : threads){
        thr.join();
    }
}

// 单个进程获得多次实例
void Test2(){
    // 预期结果，打印出相同的地址，之间换行符分隔
    print_singleton_instance();
    print_singleton_instance();
}

int main(){
    cout << "Test1 begins: " << endl;
    Test1();
    cout << "Test2 begins: " << endl;
    Test2();
    return 0;
}
```
## 方法五： 最终方案，最简&显式控制实例销毁
在上述的四种方法中，除了第四种没有使用new操作符实例化对象以外，其余三种都使用了；

​ 我们一般的编程观念是，new操作是需要和delete操作进行匹配的；是的，这种观念是正确的。在上述的实现中，是添加了一个destoryInstance的static函数，这也是最简单，最普通的处理方法了；但是，很多时候，我们是很容易忘记调用destoryInstance函数，就像你忘记了调用delete操作一样。由于怕忘记delete操作，所以就有了智能指针；那么，在单例模型中，没有“智能单例”，该怎么办？

​ 在实际项目中，特别是客户端开发，其实是不在乎这个实例的销毁的。因为，全局就这么一个变量，全局都要用，它的生命周期伴随着软件的生命周期，软件结束了，它也就自然而然的结束了，因为一个程序关闭之后，它会释放它占用的内存资源的，所以，也就没有所谓的内存泄漏了。

​但是，有以下情况，是必须需要进行实例销毁的：在类中，有一些文件锁了，文件句柄，数据库连接等等，这些随着程序的关闭而不会立即关闭的资源，必须要在程序关闭前，进行手动释放；

​ 在代码实现部分的第四种方法能满足第二个条件，但是无法满足第一个条件。好了，接下来，就介绍一种方法，这种方法也是我从网上学习而来的，代码实现如下：

```c++
#include <iostream>
#include <thread>
#include <vector>
using namespace std;

class Singleton
{
private:
    Singleton(){}
    static Singleton* m_pInstance;

    // **重点在这**
    class GC // 类似Java的垃圾回收器
    {
    public:
        ~GC(){
            // 可以在这里释放所有想要释放的资源，比如数据库连接，文件句柄……等等。
            if(m_pInstance != NULL){
                cout << "GC: will delete resource !" << endl;
                delete m_pInstance;
                m_pInstance = NULL;
            }
        };
    };

    // 内部类的实例
    static GC gc;

public:
    static Singleton* getInstance(){
        return m_pInstance;
    }
};


Singleton* Singleton::m_pInstance = new Singleton();
Singleton::GC Singleton::gc;

void print_instance(){
    Singleton* obj1 = Singleton::getInstance();
    cout << obj1 << endl;
}

// 多线程获取单例
void Test1(){
    // 预期输出：相同的地址，中间可能缺失换行符，属于正常现象
    vector<thread> threads;
    for(int i = 0; i < 10; ++i){
        threads.push_back(thread(print_instance));
    }

    for(auto& thr : threads){
        thr.join();
    }
}

// 单线程获取单例
void Test2(){
    // 预期输出：相同的地址，换行符分隔
    print_instance();
    print_instance();
    print_instance();
    print_instance();
    print_instance();
}

int main()
{
    cout << "Test1 begins: " << endl;
    cout << "预期输出：相同的地址，中间可以缺失换行（每次运行结果的排列格式通常不一样）。" << endl;
    Test1();
    cout << "Test2 begins: " << endl;
    cout << "预期输出：相同的地址，每行一个。" << endl;
    Test2();
    return 0;
}
```

在程序运行结束时，系统会调用Singleton的静态成员GC的析构函数，该析构函数会进行资源的释放，而这种资源的释放方式是在程序员“不知道”的情况下进行的，而程序员不用特别的去关心，使用单例模式的代码时，不必关心资源的释放。

​ 那么这种实现方式的原理是什么呢？由于程序在结束的时候，系统会自动析构所有的全局变量，系统也会析构所有类的静态成员变量，因为静态变量和全局变量在内存中，都是存储在静态存储区的，所有静态存储区的变量都会被释放。

### 巨人的肩膀

[Singleton模式 C++实现](https://blog.csdn.net/huhaijing/article/details/51756225)


