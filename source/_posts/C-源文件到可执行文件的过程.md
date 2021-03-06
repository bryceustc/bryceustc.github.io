---
title: C++源文件到可执行文件的过程
date: 2020-04-01 17:41:28
tags: C++
categories: C++
---

对于C/C++编写的程序，从源代码到可执行文件，一般经过下面四个步骤：

1、预处理：条件编译，头文件包含，宏替换的处理，生成.i文件。

2、编译：将预处理后的文件转换成汇编语言，生成.s文件

3、汇编：汇编变为目标代码(机器代码)生成.o的文件

4、链接：连接目标代码,生成可执行程序
<!--more-->
程序的基本流程如图：

![](1.jfif)

下面以一个简单的代码执行上述的四个过程。

```c++
#include <iostream>
using namespace std;

#define pi 3.14

int main() {

    cout << "hello world" << 1 + pi << endl;
    return 0;
}
```

#### 预处理阶段
```
g++ -E test.cc -o test.i
```

预处理主要包含下面的内容：

- 对所有的“#define”进行宏展开；
- 处理所有的条件编译指令，比如“#if”,“#ifdef”，“#elif”，“#else”,“#endif”
- 处理“#include”预编译指令，将被包含的文件插入到该预编译指令的位置。这个过程是递归的，也就是说被包含的文件可能还包含其他文件
- 删除所有的注释“//”和“/**/”
- 添加行号和文件标识
- 保留所有的“#pragma”编译器指令

注意：
- 若用<>扩起文件则在系统的INCLUDE目录中寻找文件
- 若用""扩起的文件则在当前目录中寻找文件

如下是替换之后的部分文件。

![](1.jpg)

可以看到文件行数暴增，而且后面的部分没有发生什么变化，前面进行了替换，而且对宏进行了替换。

#### 编译

将预处理完的文件（.i文件）进行一系列词法分析、语法分析、语义分析及优化后，产生相应的汇编代码文件(.s文件)。
```
g++ -S test.i
```
上述命令会生成test.s文件，内容如下，为汇编指令
![](2.jpg)

#### 汇编

汇编是将汇编代码转变成机器可以执行的代码，每一个汇编语句几乎都对应一条机器指令。最终产生目标文件(.o或.obj文件)。
```
g++ -c test.s
```
这一步生成了.o文件，用vim打开为二进制机器码。
![](3.jpg)
#### 链接

连接目标代码,生成可执行程序。
```
g++ test.o -o test #  -o用于指定生成的文件名，如果不指定-o的话，则会生成a.out文件
```
生成二进制文件内容:
![](4.jpg)

**程序库**

可复用的代码，一般编译成程序库来使用，可以分为两种：

1. 静态链接库，在程序编译阶段被链接进可执行程序。一般为.a结尾
2. 动态链接库，不连接进可执行程序，程序记录需要的动态库，运行的时候才搜索加载。采用动态编译库的程序，编译后生成的可执行文件更为短小。一般为.so结尾。
3. 程序启动后需要搜索动态链接库，默认在/lib、/usr/lib等路径下搜索动态库，还可以通过修改LD_LIBRARY_PATH环境变量来指定动态库搜索路径。
4. 静态库和动态库都是由.o文件创建的。

