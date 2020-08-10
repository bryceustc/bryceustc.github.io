---
title: 使用gdb调试core文件
date: 2020-08-10 17:31:55
tags: Linux
categories: Linux
---
### 1.core文件

实习工作时经常听到一个概念是程序core掉了，需要定位解决，这里说的大部分是指对应程序由于各种异常或者bug导致在运行过程中异常退出或者中止，并且在满足一定条件下会产生一个叫做core的文件。

通常情况下，core文件会包含了程序运行时的内存，寄存器状态，堆栈指针，内存管理信息还有各种函数调用堆栈信息等，我们可以理解为是程序工作当前状态存储生成第一个文件，许多的程序出错的时候都会产生一个core文件，通过工具分析这个文件，我们可以定位到程序异常退出的时候对应的堆栈调用等信息，找出问题所在并进行及时解决。

使用 gdb 调试 core 文件，可以帮助我们快速定位程序出现段错误的位置。当然，可执行程序编译时应加上 -g 编译选项，生成调试信息。

当程序访问非法内存会产生段错误，产生段错误的常见情况有：

（1）访问不存在的内存地址；

（2）访问系统保护的内存地址；

（3）数组访问越界等。
<!--more-->
### 2. 配置操作系统使其产生core文件

首先通过ulimit命令查看一下系统是否配置支持了dump core的功能。通过ulimit -c或ulimit -a，可以查看core file大小的配置情况，如果为0，则表示系统关闭了dump core。可以通过ulimit -c unlimited来打开，或者 ulimit -c 1024 设置相应的core文件大小。**若发生了段错误，但没有core dump，是由于系统禁止core文件的生成。**

注意：这样设置只对当前shell进程有效

### 3. 利用gdb进行coredump的定位分析

**test.cc的源码**
```c++
#include <stdio.h>
void core_test() {
  int i = 0;
  scanf("%d", i) /*should have used & to get addr other than access directly*/
  printf("%d/n",i);
}

void core_test2() {
  char *ptr = "hello world";
  *prt = 0;
}
int main()
{
 core_test1();
 return 0;
}

```

**编译执行**
```shell
g++ test.cc -g -o test
```
-g 在编译的时候，产生调试信息

-o 制定目标名称,缺省的时候, gcc 编译出来的文件是a.out

**运行程序，自动生成core文件**
```
./test
```
可以看到，当输入12的时候，系统提示段错误并且core dumped

![](1.jpg)

**gdb调试core文件**

使用flie 命令确认是否core文件格式

![](2.jpg)

使用gdb调试
```
gdb test core
```

![](3.jpg)

从红色方框截图可以看到，程序中止是因为信号11，且从bt(backtrace)命令（或者where）可以看到函数的调用栈，即程序执行到test.cc的第4行，且里面调用scanf 函数，而该函数其实内部会调用_IO_vfscanf_internal()函数。

注意记住几个常用的gdb命令：

l(list) ，显示源代码，并且可以看到对应的行号；

b(break)x, x是行号，表示在对应的行号位置设置断点；

p(print)x, x是变量名，表示打印变量x的值

r(run), 表示继续执行到断点的位置

n(next),表示执行下一步

c(continue),表示继续执行

q(quit)，表示退出gdb

![](5.jpg)

### 4. coredump的一些原因

1、内存访问越界

a) 由于使用错误的下标，导致数组访问越界

b) 搜索字符串时，依靠字符串结束符来判断字符串是否结束，但是字符串没有正常的使用结束符

c) 使用strcpy, strcat, sprintf, strcmp, strcasecmp等字符串操作函数，将目标字符串读/写爆。应该使用strncpy, strlcpy, strncat, strlcat, snprintf, strncmp, strncasecmp等函数防止读写越界。

2、多线程程序使用了线程不安全的函数。

3、多线程读写的数据未加锁保护。对于会被多个线程同时访问的全局数据，应该注意加锁保护，否则很容易造成core dump

4、非法指针

a) 使用空指针

b) 随意使用指针转换。一个指向一段内存的指针，除非确定这段内存原先就分配为某种结构或类型，或者这种结构或类型的数组，否则不要将它转换为这种结构或类型的指针，而应该将这段内存拷贝到一个这种结构或类型中，再访问这个结构或类型。这是因为如果这段内存的开始地址不是按照这种结构或类型对齐的，那么访问它时就很容易因为bus error而core dump.

5、堆栈溢出.不要使用大的局部变量（因为局部变量都分配在栈上），这样容易造成堆栈溢出，破坏系统的栈和堆结构，导致出现莫名其妙的错误。


### 5. 附注

**一些常用signal的含义**

SIGABRT：调用abort函数时产生此信号。进程异常终止。

SIGBUS：指示一个实现定义的硬件故障。

SIGEMT：指示一个实现定义的硬件故障。EMT这一名字来自PDP-11的emulator trap 指令。

SIGFPE：此信号表示一个算术运算异常，例如除以0，浮点溢出等。

SIGILL：此信号指示进程已执行一条非法硬件指令。4.3BSD由abort函数产生此信号。SIGABRT现在被用于此。

SIGIOT：这指示一个实现定义的硬件故障。IOT这个名字来自于PDP-11对于输入／输出TRAP(input/outputTRAP)指令的缩写。系统V的早期版本，由abort函数产生此信号。SIGABRT现在被用于此。

SIGQUIT：当用户在终端上按退出键（一般采用Ctrl-/）时，产生此信号，并送至前台进

程组中的所有进程。此信号不仅终止前台进程组（如SIGINT所做的那样），同时产生一个core文件。

SIGSEGV：指示进程进行了一次无效的存储访问。名字SEGV表示“段违例（segmentationviolation）”。

SIGSYS：指示一个无效的系统调用。由于某种未知原因，进程执行了一条系统调用指令，但其指示系统调用类型的参数却是无效的。

SIGTRAP：指示一个实现定义的硬件故障。此信号名来自于PDP-11的TRAP指令。

SIGXCPUSVR4和4.3+BSD支持资源限制的概念。如果进程超过了其软C P U时间限制，则产生此信号。

SIGXFSZ：如果进程超过了其软文件长度限制，则SVR4和4.3+BSD产生此信号。

 

** Core_pattern的格式**

可以在core_pattern模板中使用变量还很多，见下面的列表：

%% 单个%字符

%p 所dump进程的进程ID

%u 所dump进程的实际用户ID

%g 所dump进程的实际组ID

%s 导致本次core dump的信号

%t core dump的时间 (由1970年1月1日计起的秒数)

%h 主机名

%e 程序文件名


### 巨人的肩膀

[gdb调试coredump（使用篇）](https://zhuanlan.zhihu.com/p/46605905)

[coredump简介与coredump原因总结](https://blog.csdn.net/newnewman80/article/details/8173770)

[Linux 下使用 gdb 调试 core 文件](https://blog.csdn.net/K346K346/article/details/48344263)

[Linux的gcc和g++的区别](https://blog.csdn.net/bit_clearoff/article/details/53965514)

[g++参数介绍](https://www.cnblogs.com/lidan/archive/2011/05/25/2239517.html)