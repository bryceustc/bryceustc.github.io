---
title: C++ 分割字符串套路
date: 2020-05-24 19:37:14
tags: C++
categories: C++
---
做题时经常会遇到字符串分割（类似python的split函数）。C++并没有提供关于这个函数的功能，所以要自己实现，现整理一些套路，以后直接套用。

1. 直接暴力分割
比如分割字符串s="hello world hello C++"，要按照空格分割出单词，可以直接暴力模拟
<!--more-->
```c++
string str = "hello world hello c++";
vector<string> split(const string &str, const char pattern)
{
    vector<string> res;
    string temp;
    int n = str.size();
    for (int i=0;i<n;i++)
    {
        if (str[i]== ' ')  // 如果以逗号分割，改为','即可
        {
            temp += str[i];
        }
        else
        {
            res.push_back(temp);
            temp = "";
        }
    }
    // 注意最后一个单词
    res.push_back(temp);
    return res;
}
```

2. 利用stringstream进行字符串分割
stringstream为字符串输入输出流，继承自iostream，灵活地使用stringstream流可以完成很多字符串处理功能
```c++
vector<string> split(const string &str, const char pattern)
{
    vector<string> res;
    stringstream ss(str);   //读取str到字符串流中
    string temp;
    //使用getline函数从字符串流中读取,遇到分隔符时停止,和从cin中读取类似，注意,getline默认是可以读取空格的

    // 以空格分割可以这样写
    while(ss >> temp)
    {
        res.push_back(temp);
    }
    // 以逗号分割
    while(getline(input, temp, ',')) // pattern为分割标示符
    {
        res.push_back(temp);
    }
    return res;
}
```
3. 利用find和substr这两个函数来实现
```c++
vector<string> split(const string &str, const string &pattern)
{
    vector<string> res;
    if(str == "")
        return res;
    //在字符串末尾也加入分隔符，方便截取最后一段
    // 如果pattern为" " 为空格分割，","为逗号分割
    string strs = str + pattern;
    int pos = strs.find(pattern); // find返回分割符的下标

    while(pos != strs.npos) 
    // npos可以表示string的结束位置，是string::type_size类型的，也就是find（）返回的类型。find函数在找不到指定值得情况下会返回string::npos
    {
        string temp = strs.substr(0, pos);
        res.push_back(temp);
        //去掉已分割的字符串,在剩下的字符串中进行分割
        strs = strs.substr(pos+1, strs.size());
        pos = strs.find(pattern);
    }

    return res;
}
```

PS：
如果用Python split函数更方便，可以直接改用python
```python
s = "hello world hello c++"

s = s.split()
# s= ['hello', 'world','hello','c++']

# str.split(str="", num=string.count(str)).
# str -- 分隔符，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等。
# num -- 分割次数。默认为 -1, 即分隔所有。
```
