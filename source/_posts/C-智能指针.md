---
title: 智能指针详解
date: 2020-04-01 15:31:47
tags: C++
categories: C++
---

智能指针分为三种：shared_ptr, unique_ptr, weak_ptr。本文就是详细介绍一下智能指针，作为笔记，方便理解。

智能指针的作用是动态分配内存，避免内存泄漏。
<!--more-->
    PS:
    内存泄漏：
        程序没有释放已经不再使用的内存，由于设计错误，导致在释放该段内存
        之前就失去了对该段内存的控制，因此这段内存一直被占用，无法释放，造
        成空间的浪费。

    内存溢出：
        要求分配的内存超过了系统所能给我的，系统不能满足需求。内存泄漏的堆
        积如果不及时处理最终会导致内存溢出。

## shared_ptr介绍

shared_ptr是一个引用计数智能指针，用于共享对象的所有权，也就是说它允许多个指针指向同一个对象

### shared_ptr的简单使用：

```c++
#include <iostream>
#include <memory>
using  namespace std;

class Example
{
    public:
        Example() : e(1) { cout << "Example Constructor..." << endl; }
        ~Example() { cout << "Example Destructor..." << endl; }

        int e;
};

int main() {

    shared_ptr<Example> pInt(new Example());
    cout << (*pInt).e << endl;
    cout << "pInt引用计数: " << pInt.use_count() << endl;

    shared_ptr<Example> pInt2 = pInt;
    cout << "pInt引用计数: " << pInt.use_count() << endl;
    cout << "pInt2引用计数: " << pInt2.use_count() << endl;
    system("pause");
    return 0;
}
```
程序输出如下：
```c++
Example Constructor...
pInt: 1
pInt引用计数: 1
pInt引用计数: 2
pInt2引用计数: 2
Example Destructor...
```
从上面这段代码中，我们对shared_ptr指针有了一些直观的了解。一方面，跟STL中大多数容器类型一样，shared_ptr也是模板类，因此在创建shared_ptr时需要指定其指向的类型。另一方面，正如其名一样，shared_ptr指针允许让多个该类型的指针共享同一堆分配对象。同时shared_ptr使用经典的“引用计数”方法来管理对象资源，每个shared_ptr对象关联一个共享的引用计数。

对于shared_ptr在拷贝和赋值时的行为，《C++Primer第五版》中有详细的描述：

```
每个shared_ptr都有一个关联的计数值，通常称为引用计数。无论何时我们拷贝一个shared_ptr，计数器都会递增。

例如，当用一个shared_ptr初始化另一个shred_ptr，或将它当做参数传递给一
个函数以及作为函数的返回值时，它所关联的计数器就会递增。

当我们给shared_ptr赋予一个新值或是shared_ptr被销毁（例如一个局部的
shared_ptr离开其作用域）时，计数器就会递减。一旦一个shared_ptr的计数
器变为0，它就会自动释放自己所管理的对象。
```

对比我们上面的代码可以看到：当我们将一个指向Example对象的指针交给pInt管理后，其关联的引用计数为1。接下来，我们用pInt初始化pInt2，两者关联的引用计数值增加为2。随后，函数结束，pInt和PInt2相继离开函数作用于，相应的引用计数值分别自减1最后变为0，于是Example对象被自动释放（调用其析构函数）。

### shared_ptr的常见用法

#### 1、创建shared_ptr实例

最安全和高效的方法是调用make_shared库函数，该函数会在堆中分配一个对象并初始化，最后返回指向此对象的share_ptr实例。如果你不想使用make_ptr，也可以先明确new出一个对象，然后把其原始指针传递给share_ptr的构造函数。

```c++
int main() {

    // 传递给make_shared函数的参数必须和shared_ptr所指向类型的某个构造函数相匹配
    shared_ptr<string> pStr = make_shared<string>(10, 'a');
    cout << *pStr << endl;  //  aaaaaaaaaa

    int *p = new int(5);
    shared_ptr<int> pInt(p);
    cout << *pInt << endl;  // 5
}
```

#### 2、访问所指对象

shared_ptr的使用方式与普通指针的使用方式类似，既可以使用解引用操作符*获得原始对象进而访问其各个成员，也可以使用指针访问符->来访问原始对象的各个成员。

#### 3、拷贝和赋值操作

我们可以用一个shared_ptr对象来初始化另一个share_ptr实例，该操作会增加其引用计数值。

```c++
int main() {
    shared_ptr<string> pStr = make_shared<string>(10, 'a');
    cout << pStr.use_count() << endl;  //  1

    shared_ptr<string> pStr2(pStr);
    cout << pStr.use_count() << endl;  //  2
    cout << pStr2.use_count() << endl;  //  2
}
```
如果shared_ptr实例p和另一个shared_ptr实例q所指向的类型相同或者可以相互转换，我们还可以进行诸如p = q这样赋值操作。该操作会递减p的引用计数值，递增q的引用计数值。

例如：
```c++
class Example
{
public:
    Example(string n) : name(n) { cout << n << " constructor..." << endl; }
    ~Example() { cout << name << " destructor..." << endl; }

    string name;
};

int main() {

    shared_ptr<Example> pStr = make_shared<Example>("a object");
    shared_ptr<Example> pStr2 = make_shared<Example>("b object");
    cout << pStr.use_count() << endl;
    cout << pStr2.use_count() << endl;

    pStr = pStr2;   // 此后pStr和pStr指向相同对象
    cout << pStr->name << endl;
    cout << pStr2->name << endl;
    system("pause");
    return 0;
}
```

输出如下：
```c++
a object constructor...
b object constructor...
1
1
a object destructor...
b object
b object
b object destructor...
```

#### 4、检查引用计数

shared_ptr提供了两个函数来检查其共享的引用计数值，分别是unique()和use_count()。

在前面，我们已经多次使用过use_count()函数，该函数返回当前指针的引用计数值。值得注意的是use_count()函数可能效率很低，应该只把它用于测试或调试。

unique()函数用来测试该shared_ptr是否是原始指针唯一拥有者，也就是use_count()的返回值为1时返回true，否则返回false。

示例：
```c++
int main() {
    shared_ptr<string> pStr = make_shared<string>(10, 'a');
    cout << pStr.unique() << endl;  // true

    shared_ptr<string> pStr2(pStr);
    cout << pStr2.unique() << endl; // false;
}
```

## weak_ptr介绍

在正式介绍weak_ptr之前，我们先来回忆一下shared_ptr的一些知识。我们知道shared_ptr是采用引用计数的智能指针，多个shared_ptr实例可以指向同一个动态对象，并维护了一个共享的引用计数器。对于引用计数法实现的计数，总是避免不了循环引用（或环形引用）的问题，shared_ptr也不例外。

我们先来看看下面这个例子：
```c++
#include <iostream>
#include <memory>
#include <vector>
using namespace std;

class ClassB;

class ClassA
{
public:
    ClassA() { cout << "ClassA Constructor..." << endl; }
    ~ClassA() { cout << "ClassA Destructor..." << endl; }
    shared_ptr<ClassB> pb;  // 在A中引用B
};

class ClassB
{
public:
    ClassB() { cout << "ClassB Constructor..." << endl; }
    ~ClassB() { cout << "ClassB Destructor..." << endl; }
    shared_ptr<ClassA> pa;  // 在B中引用A
};

int main() {
    shared_ptr<ClassA> spa = make_shared<ClassA>();
    shared_ptr<ClassB> spb = make_shared<ClassB>();
    spa->pb = spb;
    spb->pa = spa;
    // 函数结束，思考一下：spa和spb会释放资源么？
    return 0;
}
```
输出：
```c++
ClassA Constructor...
ClassB Constructor...
Program ended with exit code: 0
```
从上面代码中，ClassA和ClassB间存在着循环引用，从运行结果中我们可以看到：当main函数运行结束后，spa和spb管理的动态资源并没有得到释放，产生了内存泄漏。

为了解决类似这样的问题，C++11引入了weak_ptr，来打破这种循环引用。

weak_ptr是为了配合shared_ptr而引入的一种智能指针，它指向一个由shared_ptr管理的对象而不影响所指对象的生命周期，也就是将一个weak_ptr绑定到一个shared_ptr不会改变shared_ptr的引用计数。不论是否有weak_ptr指向，一旦最后一个指向对象的shared_ptr被销毁，对象就会被释放。

### weak_ptr用法

#### 1、创建weak_ptr实例
当我们创建一个weak_ptr时，需要用一个shared_ptr实例来初始化weak_ptr，由于是弱共享，weak_ptr的创建并不会影响shared_ptr的引用计数值。

示例:
```c++
int main() {
    shared_ptr<int> sp(new int(5));
    cout << "创建前sp的引用计数：" << sp.use_count() << endl;    // use_count = 1

    weak_ptr<int> wp(sp);
    cout << "创建后sp的引用计数：" << sp.use_count() << endl;    // use_count = 1
    return 0;
}
```
#### 2、判断weak_ptr指向对象是否存在

既然weak_ptr并不改变其所共享的shared_ptr实例的引用计数，那就可能存在weak_ptr指向的对象被释放掉这种情况。这时，我们就不能使用weak_ptr直接访问对象。那么我们如何判断weak_ptr指向对象是否存在呢？C++中提供了lock函数来实现该功能。如果对象存在，lock()函数返回一个指向共享对象的shared_ptr，否则返回一个空shared_ptr。

示例：
```c++
class A
{
public:
    A() : a(3) { cout << "A Constructor..." << endl; }
    ~A() { cout << "A Destructor..." << endl; }

    int a;
};

int main() {
    shared_ptr<A> sp(new A());
    weak_ptr<A> wp(sp);
    //sp.reset();

    if (shared_ptr<A> pa = wp.lock())
    {
        cout << pa->a << endl;
    }
    else
    {
        cout << "wp指向对象为空" << endl;
    }
    return 0;
}
```
如果把sp.reset()这行的注释去掉会导致wp指向对象为空。

weak_ptr还提供了expired()函数来判断所指对象是否已经被销毁。

示例：
```c++
class A
{
public:
    A() : a(3) { cout << "A Constructor..." << endl; }
    ~A() { cout << "A Destructor..." << endl; }

    int a;
};

int main() {
    shared_ptr<A> sp(new A());
    weak_ptr<A> wp(sp);
    sp.reset(); // 此时sp被销毁
    cout << wp.expired() << endl;  // true表示已被销毁，否则为false
    return 0;
}
```
输出如下：
```c++
A Constructor...
A Destructor...
1
```

#### 3、具体使用weak_ptr
weak_ptr并没有重载operator->和operator *操作符，因此不可直接通过weak_ptr使用对象，典型的用法是调用其lock函数来获得shared_ptr示例，进而访问原始对象.

下面使用weak_ptr来改造最前面的代码，打破循环引用问题。
```c++
class ClassB;

class ClassA
{
public:
    ClassA() { cout << "ClassA Constructor..." << endl; }
    ~ClassA() { cout << "ClassA Destructor..." << endl; }
    weak_ptr<ClassB> pb;  // 在A中引用B
};

class ClassB
{
public:
    ClassB() { cout << "ClassB Constructor..." << endl; }
    ~ClassB() { cout << "ClassB Destructor..." << endl; }
    weak_ptr<ClassA> pa;  // 在B中引用A
};

int main() {
    shared_ptr<ClassA> spa = make_shared<ClassA>();
    shared_ptr<ClassB> spb = make_shared<ClassB>();
    spa->pb = spb;
    spb->pa = spa;
    // 函数结束，思考一下：spa和spb会释放资源么？
}
```

从运行结果可以看到spa和spb指向的对象都得到释放。

## unique_ptr介绍

unique是独特的、唯一的意思，故名思议，unique_ptr可以“独占”地拥有它所指向的对象，它提供一种严格意义上的所有权。这一点和我们前面介绍的shared_ptr类型指针有很大的不同：shared_ptr允许多个指针指向同一对象，而unique_ptr在某一时刻只能有一个指针指向该对象。unique_ptr保存指向某个对象的指针，当它本身被删除或者离开其作用域时会自动释放其指向对象所占用的资源。

下图演示了两个 unique_ptr 实例之间的所有权转换。

![](unique_ptr.png)

### unique_ptr的基本特性

#### 1、创建unique_ptr
示例：
```c++
int main() {
    // 创建一个unique_ptr实例
    // unique_ptr<int> pInt(new int(5));
    unique_ptr<int> pInt = make_unique<int>(5);
    cout << *pInt;
}
```

#### 2、无法进行复制构造和赋值操作
unique_ptr没有copy构造函数，不支持普通的拷贝和赋值操作。
```c++
int main() {
    // 创建一个unique_ptr实例
    unique_ptr<int> pInt = make_unique<int>(5);
    unique_ptr<int> pInt2(pInt);    // 报错
    unique_ptr<int> pInt3 = pInt;   // 报错
}
```

#### 3、可以进行移动构造和移动赋值操作

unique_ptr虽然没有支持普通的拷贝和赋值操作，但却提供了一种移动机制来将指针的所有权从一个unique_ptr转移给另一个unique_ptr。如果需要转移所有权，可以使用std::move()函数。

示例：
```c++
int main() {
    unique_ptr<int> pInt = make_unique<int>(5);
    unique_ptr<int> pInt2 = std::move(pInt);    // 转移所有权
    //cout << *pInt << endl; // 出错，pInt为空
    cout << *pInt2 << endl;
    unique_ptr<int> pInt3(std::move(pInt2));
}
```

#### 4、可以返回unique_ptr

unique_ptr不支持拷贝操作，但却有一个例外：可以从函数中返回一个unique_ptr。

示例：
```c++
unique_ptr<int> clone(int p)
{
    unique_ptr<int> pInt = make_unique<int>(p);
    return pInt;    // 返回unique_ptr
}

int main() {
    int p = 5;
    unique_ptr<int> ret = clone(p);
    cout << *ret << endl;
}
```

### unique_ptr使用场景

#### 1、为动态申请的资源提供异常安全保证
我们先来看看下面这一段代码：
```c++
void Func()
{
    int *p = new int(5);

    // ...（可能会抛出异常）

    delete p;
}
```
这是我们传统的写法：当我们动态申请内存后，有可能我们接下来的代码由于抛出异常或者提前退出（if语句）而没有执行delete操作。

解决的方法是使用unique_ptr来管理动态内存，只要unique_ptr指针创建成功，其析构函数都会被调用。确保动态资源被释放。
```c++
void Func()
{
    unique_ptr<int> pInt = make_unique<int>(5);

    // ...（可能会抛出异常）
}
```
#### 2、返回函数内动态申请资源的所有权
```c++
unique_ptr<int> Func(int p)
{
    unique_ptr<int> pInt = make_unique<int>(p);
    return pInt;    // 返回unique_ptr
}

int main() {
    int p = 5;
    unique_ptr<int> ret = Func(p);
    cout << *ret << endl;
    // 函数结束后，自动释放资源
}
```

#### 3、在容器中保存指针
```c++
int main() {
    vector<unique_ptr<int>> vec;
    unique_ptr<int> p= make_unique<int>(5);
    vec.push_back(std::move(p));    // 使用移动语义
}
```

#### 4、管理动态数组
标准库提供了一个可以管理动态数组的unique_ptr版本。

可以使用make_unique来创建一个到数组的 unique_ptr，但不能使用 make_unique 来初始化数组元素
```c++
int main() {
    // unique_ptr<int[]> p(new int[5] {1, 2, 3, 4, 5});

    // Create a unique_ptr to an array of 5 integers.
    auto p = make_unique<int[]>(5);

    // Initialize the array.
    for (int i = 0; i < 5; ++i)
    {
        p[i] = i;
        wcout << p[i] << endl;
    }
    p[0] = 0;   // 重载了operator[]
}
```

#### 5、作为auto_ptr的替代品