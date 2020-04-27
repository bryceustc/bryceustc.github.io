---
title: C++函数返回局部变量的问题
date: 2020-04-08 15:16:47
tags: C++
categories: C++
---
### 常见栈内变量
一般来说，在函数内对于存在栈上的局部变量的作用域只在函数内部，在函数返回后，局部变量的内存已经释放。因此，如果函数返回的是局部变量的值，不涉及地址，程序不会出错；但是如果返回的是局部变量的地址（指针）的话，就造成了野指针，程序运行会出错，因为函数只是把指针复制后返回了，但是指针指向的内容已经被释放了，这样指针指向的内容就是不可预料的内容，调用就会出错。
<!--more-->
```c++
#include <iostream>
using namespace std;
int fun1() {
    int i = 1;
    return i; // OK.
}
int* fun2() {
    int i = 2;
    int* ip = &i;
    return ip; // Wrong!
}
int main() {
    int r1 = fun1();
    cout << r1 << endl; // 1
    int* r2 = fun2();
    cout << *r2 << endl; 
    // 这里有可能可以打印出结果：2，看似正确的，但其实是有问题的。
    //这是因为相应的内存还未被覆盖，但这块内存已经是自由的、不被保护的了。
    return 0;
}
```

### 字符串

```c++
#include <iostream>
using namespace std;
char* fun3() {
    char* s = "Hello";
    return s; // OK.
}
char* fun4() {
    char s[] = "Hello";
    return s; // Wrong!
}
int main() {
    char* r3 = fun3();
    cout << r3 << endl; // Hello
    char* r4 = fun4();
    cout << r4 << endl; // 内存已经无效的了。打印出乱码。
    return 0;
}
```

### 静态变量
如果函数的返回值非要是一个局部变量地址，可以把局部变量声明为static静态变量。这样变量存储在静态存储区，程序运行过程中一直存在。

```c++
#include <iostream>
using namespace std;
int* fun5() {
    static int i = 5;
    return &i; // OK.
}
char* fun6() {
    static char s[] = "Hello";
    return s; // OK.
}
int main() {
    int* r5 = fun5();
    cout << *r5 << endl; // 5
    char* r6 = fun6();
    cout << r6 << endl; // Hello
    return 0;
}
```

### 数组

数组是不能作为函数的返回值的。因为编译器会把数组名认为是局部变量（数组）的地址。返回一个数组，实际上是返回指向这个数组首地址的指针。函数结束后，数组作为局部变量被释放，这个指针则变成了野指针。同1的fun2()及2的fun4()（字符数组）。但是声明数组是静态的，然后返回是可以的.

```c++
#include <iostream>
using namespace std;
int* fun7() {
    int a[3] = {1, 2, 3};
    return a; // Wrong!
}
int* fun8() {
    static int a[3] = {1, 2, 3};
    return a; // OK.
}
int main() {
    int* r7 = fun7();
    cout << *r7 << endl; // 内存已经是无效的了。
    int* r8 = fun8();
    cout << *r8 <<endl; // 1
    return 0;
}
```

### 堆内变量
函数返回指向存储在堆上的变量的指针是可以的。但是，程序员要自己负责在函数外释放（free/delete）分配（malloc/new）在堆上的内存。

```c++
#include <iostream>
using namespace std;
char* fun9() {
    char* s = (char*) malloc(sizeof(char) * 100);
    return s; // OK. 但需要程序员自己释放内存。
}
int main() {
    char* r9 = NULL;
    r9 = fun9();
    strcpy(r9, "Hello");
    cout << r9 << endl; // Hello
    free(r9); // 要记得自己释放内存。
    return 0;
}
```