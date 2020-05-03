---
title: C++模板Template
date: 2020-05-03 21:03:06
tags: C++
categories: C++
---

模板是泛型编程的基础，泛型编程即以一种独立于任何特定类型的方式编写代码。

模板是创建泛型类或函数的蓝图或公式。库容器，比如迭代器和算法，都是泛型编程的例子，它们都使用了模板的概念。

每个容器都有一个单一的定义，比如 向量，我们可以定义许多不同类型的向量，比如 vector <int> 或 vector <string>。
<!--more-->

### 函数模板

函数模板可以理解为一个公式，可用来生成针对特定类型的函数版本。

####　普通函数模板
下面定义了一个名叫compare的函数模板，支持多种类型的通用比较逻辑。
```c++
template<typename T>    // 模板关键字template，后边为模板参数列表，不能为空
//  类型参数前必须使用关键字class 或者 typename，这两个没什么区别
int compare(const T& left, const T& right) {
    if (left < right) {
        return -1; 
    }
    if (right < left) {
        return 1; 
    }
    return 0;
}

compare<int>(1, 2); //使用模板函数
```
#### 成员函数模板
不仅普通函数可以定义为模板，类的成员函数也可以定义为模板。

```c++
class Printer {
public:
    template<typename T>
    void print(const T& t) {
        cout << t <<endl;
    }
};

Printer p;
p.print<const char*>("abc"); //打印abc
```
#### 为什么成员函数模板不能是虚函数(virtual)？
编译器都期望在处理类的定义的时候就能确定这个类的虚函数表的大小，如果允许有类的虚成员模板函数，那么就必须要求编译器提前知道程序中所有对该类的该虚成员模板函数的调用，而这是不可行的。 

在实例化模板类时，需要创建virtual table。
在模板类被实例化完成之前不能确定函数模板(包括虚函数模板，加入支持的话)会被实例化多少个。

普通成员函数模板无所谓，什么时候需要什么时候就给你实例化，编译器不用知道到底需要实例化多少个，虚函数的个数必须知道，否则这个类就无法被实例化(因为要创建virtual table)。因此，不支持虚函数模板。

#### 实参推断
为了方便使用，除了直接为函数模板指定类型参数之外，我们还可以让编译器从传递给函数的实参推断类型参数，这一功能被称为模板实参推断。

```c++
compare(1, 2); //推断T的类型为int
compare(1.0, 2.0); //推断T的类型为double
p.print("abc"); //推断T的类型为const char*
```

#### 当返回值类型也是参数时

当一个模板函数的返回值类型需要用另外一个模板参数表示时，你无法利用实参推断获取全部的类型参数，这时有两种解决办法：

- 返回值类型与参数类型完全无关，那么就需要显示的指定返回值类型，其他的类型交给实参推断。

        注意：此行为与函数的默认实参相同，我们必须从左向右逐一指定。
     ```c++
     template<typename T1, typename T2, typename T3>
    T1 sum(T2 v2, T3 v3) {
    return static_cast<T1>(v2 + v3);
    }

    auto ret = sum<long>(1L, 23); //指定T1, T2和T3交由编译器来推断

    template<typename T1, typename T2, typename T3>
    T3 sum_alternative(T1 v1, T2 v2) {
    return static_cast<T1>(v1 + v2);
    }
    auto ret = sum_alternative<long>(1L, 23); //error，只能从左向右逐一指定
    auto ret = sum_alternative<long,int,long>(1L,23); //ok, 谁叫你把最后一个T3作为返回类型的呢？
     ```   
- 返回值类型可以从参数类型中获得，那么把函数写成尾置返回类型的形式，就可以愉快的使用实参推断了。

    ```c++
    template<typename It>
    auto sum(It beg, It end) -> decltype(*beg)   // 尾置返回类型
    {
        decltype(*beg) ret = *beg;
        for (It it = beg+1; it != end; it++) {
            ret  = ret + *it;
        }
        return ret;
    }

    vector<int> v = {1, 2, 3, 4};
    auto s = sum(v.begin(), v.end()); //s = 10
    ```
#### 实参推断时的自动类型转换
编译器进行模板实参推断时通常不会对实参进行类型转换，只有以下几种情况例外：

- 普通对象赋值给const引用 ``int a = 0; -> const T&``
- 数组名转换为头指针 ``int a[10] = {0}; -> T*``
- 函数名转换为函数指针`` void func(int a){...} -> T*``

### 类模板

类模板也是公共逻辑的抽象，通常用来作为容器（例如：vector）或者行为（例如：clonable）的封装。

参考侯捷老师《STL源码分析》的视频理解，设计一个复数

![](1.png)

与函数模板不同，类模板不能推断实例化。所以你只能显示指定类型参数使用complex<double>  c1(2.5, 1.5)，complex<int>  c2(2, 6)，而不能让编译器自行推断complex  c1。

#### 类模板中的成员函数模板
我们还可以把类模板和函数模板结合起来，定义一个含有成员函数模板的类模板。
```c++
template<typename T>
class Printer {
public:
    explicit Printer(const T& param):t(param){}
    //成员函数模板
    template<typename U>
    void add_and_print(const U& u);
private:
    T t;
};

//注意这里要有两层template的说明
template<typename T>
template<typename U>
void Printer<T>::add_and_print(const U& u) {
    cout << t + u << endl;
}

Printer<int> p(42);
p.add_and_print(1.1); //自动推断U为double，打印出43.1
```

#### 类模板中的static成员
类模板中可以声明static成员，在类外定义的时候要增加template相关的关键词。另外，需要注意的是：每个不同的模板实例都会有一个独有的static成员对象。
```c++
// 静态成员属于类共享，不独享于某一个实例对象
template<typename T>
class Printer {
public:
    explicit Printer(const T& param):t(param){}
    static int s_value;
private:
    T t;
};

template<typename T> //注意这里的定义方式
int Printer<T>::s_value = 1;

Printer<int> pi(1);
Printer<int> pi2(1);
Printer<double> pd(1.0);
pi.s_value += 1; //pi和pi2中的改变了，pd的没改变

```
其实这个结论是显然的，static成员属于实例化后的类，不同的实例化当然有不同static成员。就像上面的例子一样，pi.s_value += 1只影响到了Printer<int>，而不会影响到Printer<double>。

#### 类模板成员函数实例化
为了节省资源，类模板实例化时并不是每个成员函数都实例化了，而是使用到了哪个成员函数，那个成员函数才实例化。
```c++
template<typename T>
class Printer {
public:
    explicit Printer(const T& param):t(param){}
    void print() {
        cout << t << endl;
    }
 private:
    T t;
 };

class empty{};

empty e;
Printer<empty> p(e); //ok
```
虽然成员函数print无法通过编译，但是因为没有使用到，也就没有实例化print，所以没有触发编译错误。

#### 类模板别名
为了简化代码，我们可以使用typedef为类模板的某个实例定义一个别名，也可以使用using语句固定一个或多个类型参数（这有点偏特化的意思了）。
```c++
typedef std::pair<int, int> PairOfInt; //ok，为std::pair<int, int>定义了一个别名
template <typename T> using WithNum = std::pair<T, int>; //ok，相当于定义了一个std::pair的偏特化

PairOfInt poi; //实际类型，std::pair<int, int>
WithNum<std::string> strs; //实际类型，std::pair<string, int>
WithNum<int> ints; //实际类型，std::pair<int, int>
```

### 成员模板

### 特化与偏特化

#### 类模板的特化与偏特化
就像函数模板重载那样，你可以通过特化（偏特化）类模板来为特定的类型指定你想要的行为。类模板的特化（偏特化）只需要模板名称相同并且特化列表<>中的参数个数与原始模板对应上即可，模板参数列表不必与原始模板相同模板名称相同。一个类模板可以有多个特化，与函数模板相同，编译器会自动实例化那个最特殊的版本。

模板特化指的是模板中指定特定的数据类型，这和泛化是不同的：

![](2.png)

模板特化也有程度之分，可以部分类型指定，称之为偏特化：

![](3.png)

![](4.png)


```c++
template<typename T> //基本模板
class S {
public:
   void info() {   
       printf("In base template\n"); 
   }
};

template<> //特化
class S<int> { 
public:
   void info() {
       printf("In int specialization\n");
   }
};

template<typename T> //偏特化
class S<T*> {
public:
   void info() {
       printf("In pointer specialization\n");
    }
};

template<typename T, typename U> //另外一个偏特化
class S<T(U)> {
public:
    void info() {
        printf("In function specialization\n");
    }
};

int func(int i) {
    return 2 * i;
}

S<float> s1;
s1.info();     //调用base模板                
S<int> s2;
s2.info();     //调用int特化版本
S<float*> s3;
s3.info();     //调用T*特化版本 
S<decltype(func)> s4;
s4.info();     //调用函数特化版本
```
提供了所有类型实参的特化是完全特化，只提供了部分类型实参或者T的类型受限（例如：T*）的特化被认为是不完整的，所以也被称为偏特化。完全特化的结果是一个实际的class，而偏特化的结果是另外一个同名的模板。

#### 类模板成员特化
除了可以特化类模板之外，还可以对类模板中的成员函数和普通静态成员变量进行特化。

```c++
template<typename T>  
class S {
public:
    void info() {
        printf("In base template\n");
    }
    static int code;
};

template<typename T>
int S<T>::code = 10;

template<>
int S<int>::code = 100;    //普通静态成员变量的int特化

template<>
void S<int>::info() {    //成员函数的int特化
     printf("In int specialization\n");
} 

S<float> s1;
s1.info();    //普通版本
printf("Code is: %d\n", s1.code);    //code = 10

S<int> s2; 
s2.info();   //int特化版本
printf("Code is: %d\n", s2.code);   //code = 100
```



