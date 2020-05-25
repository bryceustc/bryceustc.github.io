---
title: Linux Vim批量注释与反注释
date: 2020-05-20 14:23:28
tags: Linux
categories: Linux
---
在使用vim编写代码的时候，经常需要用到批量注释与反注释一段代码。在此做一下记录

多行注释：

1. 进入命令行模式，按ctrl + v进入 visual block模式

![](1.png)

2. 然后按j, 或者k选中多行，把需要注释的行标记起来
<!--more-->
![](2.png)

3. 按大写字母I，再插入注释符，例如//,按两下esc键就会全部注释了

![](3.png)

取消多行注释：

1. 进入命令行模式，按ctrl + v进入 visual block模式

2. 按字母j，或者k选中注释符号

3. 按d键就可全部取消注释
