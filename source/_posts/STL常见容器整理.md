---
title: C++STL容器整理
date: 2020-04-18 12:01:54
tags: C++
categories: C++
---
### 概述

容器是储存其他对象的对象。被储存的对象必须是同一类型。

分类：

与java的集合不同!STL容器主要被分为两个部分：

一是序列容器（是一种各元素之间有顺序关系的线性表，是一种线性结构的可序群集。顺序性容器中的每个元素均有固定的位置，除非用删除或插入的操作改变这个位置。顺序容器的元素排列次序与元素值无关，而是由元素添加到容器里的次序决定）（``forword_list,list,queue,priority_queue,stack,deque,vector,array``）。
<!--more-->
另一个是关联容器（关联式容器是非线性的树结构，更准确的说是二叉树结构。各元素之间没有严格的物理上的顺序关系，也就是说元素在容器中并没有保存元素置入容器时的逻辑顺序。但是关联式容器提供了另一种根据元素特点排序的功能，这样迭代器就能根据元素的特点“顺序地”获取元素。元素是有序的集合，默认在插入的时候按升序排列（``set,multiset,map,multimap``）！

C++11之后还有一种无序关联容器！(``unordered_map, unordered_set, unordered_multiset, unordered_multimap``)


### 序列容器

序列的要求：

```c++
X a(n,t)  //声明一个名为a的有n个t组成的序列
X(n,t)     //匿名序列（这里不做过多的解释）
X a(i,j)   //声明一个名为a的序列，并且初始化[i,j）的内容
X(i,j)      //匿名序列
v.insert()   //由于insert重载方法比较多
    v.insert(p,t)//将t插到p的前面
    v.insert(p,n,t)//将n个t插入p之前
    v.insert(p,i.j)//将区间[i,j)的元素插入到p之前
v.erase(t,k)
    v.erase(t,k)//删除他们之间的元素
    v.erase(p)//删除p指向的元素
v.clear===v.erase(begin(),end());
```

下面我们来看一下这些容器的特点！

#### 1. vector

vector是数组的一种类表示，vector表示一段连续的内存，基于数组实现，他有自动的内存管理功能!可以动态的改变vector的长度，并随着元素的增加与减小来自动改变数组大小，它提供了直接添加尾部元素或者删除元素的方法！所以它的时间是固定的！然而他要在头部与中间插入或者删除元素是线性的时间复杂度！

特点：
他可以反转序列，所以它可以反向遍历可反转序列！（基于他的rbegin,rend）

调用头文件：
```c++
#include<vector>
```

定义与初始化：

```c++
vector<int> v;//默认初始化
vector<int> v(v1);//用v1初始化v
vector<int>v(v1.begin(),v1.end());//用v1初始化v
vector<int> v(10);//定义一个大小为10的数组！
vector<int> v(10,1)//定义个全为1而且长度为10的数组
vector<int> a(b.begin(),b.begin+3); //定义了a值为b中第0个到第2个（共3个）元素
int b[7]={1,2,3,4,5,9,8};
vector<int> a(b,b+7); //从数组中获得初值
```

方法：

```c++
a.assign(b.begin(), b.begin()+3); //b为向量，将b的0~2个元素构成的向量赋给a
a.assign(4,2); //是a只含4个元素，且每个元素为2
a.back(); //返回a的最后一个元素
a.front(); //返回a的第一个元素
a[i]; //返回a的第i个元素，当且仅当a[i]存在2013-12-07
a.clear(); //清空a中的元素
a.empty(); //判断a是否为空，空则返回ture,不空则返回false
a.pop_back(); //删除a向量的最后一个元素
a.erase(a.begin()+1,a.begin()+3); //删除a中第1个（从第0个算起）到第2个元素，也就是说删除的元素从a.begin()+1算起（包括它）一直到a.begin()+3（不包括它）
a.push_back(5); //在a的最后一个向量后插入一个元素，其值为5
a.insert(a.begin()+1,5); //在a的第1个元素（从第0个算起）的位置插入数值5，如a为1,2,3,4，插入元素后为1,5,2,3,4
a.insert(a.begin()+1,3,5); //在a的第1个元素（从第0个算起）的位置插入3个数，其值都为5
a.insert(a.begin()+1,b+3,b+6); //b为数组，在a的第1个元素（从第0个算起）的位置插入b的第3个元素到第5个元素（不包括b+6），如b为1,2,3,4,5,9,8 ，插入元素后为1,4,5,9,2,3,4,5,9,8
a.size(); //返回a中元素的个数；
a.capacity(); //返回a在内存中总共可以容纳的元素个数
a.resize(10); //将a的现有元素个数调至10个，多则删，少则补，其值随机
a.resize(10,2); //将a的现有元素个数调至10个，多则删，少则补，其值为2
a.reserve(100); //将a的容量（capacity）扩充至100，也就是说现在测试a.capacity();的时候返回值是100.这种操作只有在需要给a添加大量数据的时候才  显得有意义，因为这将避免内存多次容量扩充操作（当a的容量不足时电脑会自动扩容，当然这必然降低性能） 
a.swap(b); //b为向量，将a中的元素和b中的元素进行整体性交换
a==b; //b为向量，向量的比较操作还有!=,>=,<=,>,<
```

遍历：

```c++
//下标法
int length = v.size();
for(int i=0;i<length;i++)
{
    cout<<v[i];
}
cout<<endl;
//迭代器法
vector<int>::const_iterator iterator = v.begin();
for(;iterator != v.end();iterator++)
{
    cout<<*iterator;
}
```

几种常用重要的算法，使用时需要包含头文件：
```c++
#include<algorithm>
sort(a.begin(),a.end()); //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素进行从小到大排列
reverse(a.begin(),a.end()); //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素倒置，但不排列，如a中元素为1,3,2,4,倒置后为4,2,3,1
copy(a.begin(),a.end(),b.begin()+1); //把a中的从a.begin()（包括它）到a.end()（不包括它）的元素复制到b中，从b.begin()+1的位置（包括它）开始复制，覆盖掉原有元素
find(a.begin(),a.end(),10); //在a中的从a.begin()（包括它）到a.end()（不包括它）的元素中查找10，若存在返回其在向量中的位置
```

#### 2. deque

双端队列，他的实现类似与vector,支持随机访问，但是它访问首元素的插入（push_front()）与删除（pop_front()）的时间是固定的！而且他的执行速度要比vector快很多！所以需要有大量的操作发生在序列的起始位置与结尾处，我们就要考虑用deque!

调用头文件：
```c++
 #include<deque>
```
初始化与定义已经在序列要求里面，而且方法与vector类似，只是多了push_front(),pop_front(),不做过多的阐述

#### 3. list

双向链表，list在链表中的任意一个位置插入与删除一个元素时间是固定的！但是他不能随机访问，优点是元素的快速插入与删除！从容器中插入与删除元素之后i，迭代器指向元素将不变，不会移动已有元素，只是修改链表信息。

调用头文件：
```c++
#include<list>
```

链表独有成员函数:

```c++
void sort()   //使用<运算符对链表进行排序，时间复杂度O（NlogN）
void merge(list<T,Alloc>&x)  //将x与调用链表合并，要求：两个链表必须要已经排好序！元素将保存在调用链表中，x为空，这个时间复杂度为线性！
void remove(const T &val)//删除val的所有实例
void splice(iterator pos,list<T,Alloc>x)//将链表x的内容加到pos的前面，时间复杂度为固定时间
void unique（） //去重，线性时间
```

**注意：**
algorithm头文件中有sort函数，这个list也有自己的sort函数，排序时都可以用，不过使用自带的效率会更高一点。

#### 4. forword_list

主要实现单向链表，与list类似，只需要正向迭代器，他是不可逆转容器，他功能比较少，比较简单


#### 5. queue

他是一个适配器类，可以用数组也可以用链表实现队列，它不允许随机访问元素，而且不能遍历队列！元素只能先进先出（FIFO）.

调用头文件：
```c++
#include<queue>
```

queue的定义，queue<typename> q;其中typename可以为任何类型或容器。

queue的访问，由于队列是一种先进先出的限制性数据结构，因此在STL中只能通过front()来访问队首元素，或是通过back()来访问队尾元素。

方法：

```c++
bool empty()//判断是否为空
front()//队首元素的访问
back()//队尾元素的访问
push(x)//队尾插入x
pop()//删除队首元素
```

#### 6. priority_queue

优先队列，其底层是用堆来进行实现的。在优先队列中，队首的元素一定是当前队列中优先级最高的那一个。C++中的堆默认是大顶堆。

调用头文件：
```c++
#include<queue>
```

priority_queue的定义，priority_queue<typename> q,typename可以为任意类型的元素。


priority_queue只能通过top()函数来访问队首元素(堆顶元素)，也就是优先级最高的元素。

```c++
priority_queue<int> X //大根堆,默认初始化

priority_queue<int, vector<int>, greater<int>> x  //小根堆,运用了预定义函数greater<int>！
```

以下内容摘自C++API：
```c++
包含priority_queue 的头文件是 <queue>
priority_queue类的主要成员：
priority_queue();    //默认构造函数，生成一个空的排序队列
priority_queue(const queue&);    //拷贝构造函数
priority_queue& operator=(const priority_queue &);    //赋值运算符重载
priority_queue 的私有成员：
value_type;   //priority_queue中存放的对象类型，它和priority_queue中的T类型相同
priority_queue(const Compare& comp);    //构造生成一个空的priority_queue对象，使用comp作为priority_queue的comparison
priority_queue(const value_type* first, const value_type* last);    //带有两个参数的构造 函数，使用默认的Comparison作为第三个参数
size_type;    //正整数类型，和Sequence::size_type类型一样。
bool empty() const;    //判断优先级队列是否为空，为空返回true，否则返回false
size_type size() const;    //返回优先级队列中的元素个数
const value_type& top() const();    //返回优先级队列中第一个元素的参考值。
void push(const value_type& x);    //把元素x插入到优先级队列的尾部，队列的长度加1
void pop();    //删除优先级队列的第一个值，前提是队列非空，删除后队列长度减1
```

#### 7. stack

栈，是STL中实现一个后进先出的容器.

调用头文件：
```c++
#include<queue>
```

stack的定义，stack<typename> name要使用stack需要加上头文件#include<stack>。

stack容器内元素的访问，由于栈本身就是一种后进先出的数据结构，在STL的stack中只能通过top()来访问栈顶元素

方法：
```c++
push(x)//将x压入栈中，时间复杂度O(1)
top()//获得栈顶元素，时间复杂度O(1)
pop()//将栈顶元素弹出，时间复杂度O(1)
empty()//判断栈是否为空，若为空，返回true，否则返回false，时间复杂度O(1)
size()//返回栈中元素的个数，时间复杂度O(1)
```

### 关联容器