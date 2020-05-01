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
#include<stack>
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

它运用了键值对（value-key），与java类似的map,例如hashmap，有点在于他提供了利用key快速访问功能，它的底层结构应该是一种树来实现的，所以他才有如此快的查找速度，最简单的set，他的键值对类型是一致的，而且唯一，元素默认按升序排列。map他的键值对类型不同，键是唯一的,元素默认按键的升序排列。！而muilti_sset/map 键可以不唯一。

迭代器在关联容器中对操作：
```c++
m.lower_bound(k)//返回一个迭代器，指向键不小于 k 的第一个元素
m.upper_bound(k)//返回一个迭代器，指向键大于 k 的第一个元素
m.equal_range(k)//返回一个迭代器的 pair 对象。它的 first 成员等价于 m.lower_bound(k)。而 second 成员则等价于 m.upper_bound(k)
```

#### 1. map
map 是键－值对的集合。map 类型通常可理解为关联数组：可使用键作为下标来获取一个值，正如内置数组类型一样。而关联的本质在于元素的值与某个特定的键相关联，而并非通过元素在数组中的位置来获取。

调用头文件：
```c++
#include<map>
```

定义与初始化:
```c++
map<int,string> m;    //默认为空
```

具体操作：
```c++
m.insert()
m.insert(e)//e是一个用在m上的value_key 类型的值。如果键（e.first不在m中，则插入一个值为e.second 的新元素；如果该键在m中已存在，则保持m不变。该函数返回一个pair类型对象，包含指向键为e.first的元素的map迭代器，以及一个 bool 类型的对象，表示是否插入了该元素
m.insert(begin,end)//begin和end是标记元素范围的迭代器，其中的元素必须为m.value_key 类型的键－值对。对于该范围内的所有元素，如果它的键在 m 中不存在，则将该键及其关联的值插入到 m。返回 void 类型
m.insert(iter,e)//e是一个用在m上的 value_key 类型的值。如果键（e.first）不在m中，则创建新元素，并以迭代器iter为起点搜索新元素存储的位置。返回一个迭代器，指向m中具有给定键的元素
m.count(k) //返回m中k的出现次数
m.find()   //如果m容器中存在按k索引的元素，则返回指向该元素的迭代器。如果不存在，则返回超出末端迭代器.
m.erase()  //具体与序列该方法一致！
```

#### 2. set
支持插入，删除，查找等操作，就像一个集合一样。所有的操作的都是严格在logn时间之内完成，效率非常高。set和multiset的区别是：set插入的元素不能相同，但是multiset可以相同。Set默认自动排序。使用方法类似list。

调用头文件：
```c++
#include<set>
```

set容器的定义和使用，set 容器的每个键都只能对应一个元素。元素本身就是它的key。

```c++
vector<int> v;
for(int i = 0; i < 10; ++i) 
{
    v.push_back(i);
    v.push_back(i);
}
set<int> s(v.begin(), v.end());
cout << v.size() << endl  //20个
cout << s.size() << endl;  // 10个
```
添加
```c++
set<string> s1;
s1.insert("the"); //第一种方法：直接添加
set<int> s2;
s2.insert(v.begin(), v.end());//第二中方法：通过指针迭代器
```

获取
```c++
set<int> v;
for(int i = 0; i<10; i++)
v.insert(i);
v.find(1)// 返回指向元素内容为1的指针
v.find(11)// 返回指针v.end()
v.count(1)// 存在，返回1
v.count(11)// 不存在，返回0
```

#### 3. 无序关联容器
底层结构基于哈希表，主要主要与提高添加与删除元素得速度与提高查找算法得效率！无序关联容器（unordered_set、unordered_multiset、unordered_map和 unordered_multimap）使用键和哈希表，以便能够快速存取数据。下面简要地介绍这些概念。哈希函数（hash function）将键转换为索引值。例如，如果键为string对象，哈希函数可能将其中每个字符的数字编码相加，再计算结果除以13的余数，从而得到 一个0～12的索引。而无序容器将使用13个桶（bucket）来存储string，所有索引为4的string都将存储在第4个桶中。如果您要在容器中搜索键，将对键执行哈希函数，进而只在索引对应的桶中搜索。理想情况下，应有足够多的桶，每个桶只包含为数不多的string。

```c++
X(n, hf, eq)//创建一个至少包含n个桶的空容器，并将hf用作哈希函数，将eq用作键值相等谓词。如果省略了eq，则将ke

y_equal( )用作键值相等谓词；如果也省略了hf，则将hasher( )用作哈希函数

X a(n, hf, eq)//创建一个名为a的空容器，它至少包含n个桶，并将hf用作哈希函数，将eq用作键值相等谓词。如果省略eq，则将key_equal( )用作键值相等谓词；如果也省略了hf，则将hasher( )用作哈希函数

X(i, j, n, hf, eq)//创建一个至少包含n个桶的空容器，将hf用作哈希函数，将eq用作键值相等谓词，并插入区间[i, j]中的元素。如果省略了eq，将key_equal( )用作键值相等谓词；如果省略了hf，将hasher( )用作哈希函数；如果省略了n，则包含桶数不确定

X a(i, j, n, hf, eq)//创建一个名为a的的空容器，它至少包含n个桶，将hf用作哈希函数，将eq用作键值相等谓词，并插入区间[i, j]中的元素。如果省略了eq，将key_equal( )用作键值相等谓词；如果省略了hf，
将hasher( )用作哈希函数；如果省略了n，则包含桶数不确定

b.hash_function( )//返回b使用的哈希函数

b.key_eq( )//返回创建b时使用的键值相等谓词

b.bucket_count( )//返回b包含的桶数

b.max_bucket_count ( )//返回一个上限数，它指定了b最多可包含多少个桶

b.bucket(k)//返回键值为k的元素所属桶的索引

b.bucket_size(n)//返回索引为n的桶可包含的元素数

b.begin(n)//返回一个迭代器，它指向索引为n的桶中的第一个元素

b.end(n)//返回一个迭代器，它指向索引为n的桶中的最后一个元素

b.cbegin(n)//返回一个常量迭代器，它指向索引为n的桶中的第一个元素

b.cend(n)//返回一个常量迭代器，它指向索引为n的桶中的最后一个元素

b.load_factor()//返回每个桶包含的平均元素数

b.max_load_factor()//返回负载系数的最大可能取值；超过这个值后，容器将增加桶

b.max_load_factor(z)//可能修改最大负载系统，建议将它设置为1

a.rehash(n)//将桶数调整为不小于n，并确保a.bucket_count( )> a.size( ) / a.max_load_factor( )

a.reserve(n)//等价于a.rehash(ceil(n/a.max_load_factor( )))，
其中ceil(x)返回不小于x的最小整数
```

### 小结
1.有序容器（除了list）：存储底层vector,只是添加了不同的接口！

2.deque（队列）：它不像vector 把所有的对象保存在一块连续的内存块，而是采用多个连续的存储块，并且在一个映射结构中保存对这些块及其顺序的跟踪。向deque 两端添加或删除元素的开销很小，它不需要重新分配空间。

3、list（列表）：是一个线性链表结构，它的数据由若干个节点构成，每一个节点都包括一个信息块（即实际存储的数据）、一个前驱指针和一个后驱指针。它无需分配指定的内存大小且可以任意伸缩，这是因为它存储在非连续的内存空间中，并且由指针将有序的元素链接起来。

4.后面的关联与无序关联都是用的一种树状结构！

用法与选择：

1.当数组大小未知时，和需要高效的查询功能，用vector!高效地随机存储。

2.不使用连续的内存空间,而且可以随意地进行动态操作，有大量的插入、删除操作，用list！

3.需要在两端进行push 、pop用daque！它兼顾了数组和链表的优点，它是分块的链表和多个数组的联合。所以它有被list 好的查询性能，有被vector 好的插入、删除性能。 如果你需要随即存取又关心两端数据的插入和删除，那么deque 是最佳之选。

4.需要查找或者打表可以选择map与set,当然一定条件下我们可以优先考虑用无序关联容器！