---
title: C++ string.find函数用法
date: 2020-05-20 23:23:15
tags:C++
categories: C++
---
整理一些find函数的用法，方便以后使用查看。

1.string中find()返回值是字母在母串中的位置（下标记录），如果没有找到，那么会返回一个特别的标记npos。（返回值可以看成是一个int型的数）
<!--more-->
```c++
#include <string>
#include <iostream>
using namespace std;
int main()
{
    ////find函数返回类型 size_type
    string s("1a2b3c4d5e6f7jkg8h9i1a2b3c4d5e6f7g8ha9i");
    string flag;
    int position;
    
    //find 函数 返回jk 在s 中的下标位置 
    position = s.find("jk");

    if(position !=s.npos)
    {
        cout << "postion is: "  << position << endl; //如果没找到，返回一个特别的标志c++中用npos表示，int类型的话，没找到时我这里position=-1，
    }
    else
    {
        cout << "Not found the flag" + flag << endl;
    }
    return 0;
}
```

2.返回子串出现在母串中的首次出现的位置，和最后一次出现的位置。

```c++
    flag = "c";
    position = s.find_first_of(flag);
    cout << "s.find_first_of(flag) is : " << position << endl;
    // s.find_first_of(flag) is : 5

    position = s.find_last_of(flag);
    cout << "s.find_last_of(flag) is : " << position << endl;
    //  s.find_last_of(flag) is : 25
```

3.查找某一给定位置后的子串的位置

```c++
//从字符串s 下标5开始，查找字符串b ,返回b 在s 中的下标
position=s.find("b",5);
cout<<"s.find(b,5) is : "<<position<<endl;
// s.find(b,5) is : 23
```

4.查找所有子串在母串中出现的位置

```c++
//查找s 中flag 出现的所有位置。
    flag="a";
    position=0;
    int i=1;
    while((position=s.find(flag,position))!=s.npos)
    {
        cout<<"position  "<<i<<" : "<<position<<endl;
        position++;
        i++;
    }
/*
position  1 : 1
position  2 : 21
position  3 : 36
*/
```
5.反向查找子串在母串中出现的位置，通常我们可以这样来使用，当正向查找与反向查找得到的位置不相同说明子串不唯一。

```c++
//反向查找，flag 在s 中最后出现的位置
    flag="3";
    position=s.rfind (flag);
    cout << "s.rfind (flag) :" << position << endl;
// s.rfind (flag) :24
```