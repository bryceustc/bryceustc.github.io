---
title: Win10安装WSL2的Linux子系统及环境配置
date: 2020-12-09 14:20:05
tags: Linux, 工具
categories: Linux
---
当我们在 Windows 平台上进行程序开发的时候，会遇到各种各样让人讨厌的问题，有时候还必须转到 Linux 环境下进行开发。然而，通过本文你将可以解决你在 Windows 平台上开发遇到的所有问题。
<!--more-->
### 1、Chocolate Package Manager
对于开发人员而言，搭建开发环境是所有开发环节中的第一步，然而在 Windows 环境下，各种安装工具，软件版本五花八门，而且容易下载到病毒软件，因此对于初学者来说，下载到正确的开发软件，搭建好开发环境还是有一定难度和技巧性的.

如果希望 windows 平台上能有像 linux，macos 上的那种包管理工具，能让我一个命令就下载相应的环境，并把环境的路径添加到环境变量中，无需自己手动操作，然而这样的包管理工具已经存在了，Chocolate Package Manager 恰好是在Windows环境下处理搭建开发环境最好、也是最简单的解决方式之一。

![](http://freeshow.oss-cn-beijing.aliyuncs.com/blog/20200222/111526357.png)

- 官网：https://chocolatey.org/
- 安装文档：https://chocolatey.org/install
- Chocolate Packages：https://chocolatey.org/packages

Chocolate安装与卸载可参考[这里](https://www.jianshu.com/p/b8b618484d0d)

安装好 Chocolate 后，Windows 下的所有环境，包括 node，npm，python，java，git，filezilla 等都是用这个工具安装的，所有包自此之后都不需手动管理，就算要卸载，也只是一个命令。

### 2、Windows Subsystem for Linux
如果我们希望能在 Windows 上舒舒服服地学习、使用 Linux，微软已经解决了这个问题，而且解决的很好。

适用于 Linux 的 Windows 子系统可让开发人员按原样运行 GNU/Linux 环境 - 包括大多数命令行工具、实用工具和应用程序 - 且不会产生虚拟机开销。

硬性要求：

Requirements：

- For x64 systems: Version 1903 or higher, with Build 18362 or higher.

- For ARM64 systems: Version 2004 or higher, with Build 19041 or higher.

官方安装参考教程：https://docs.microsoft.com/en-us/windows/wsl/install-win10

### 3、 zsh + oh-my-zsh 安装
zsh 官网：https://github.com/zsh-users/zsh

oh-my-zsh 官网：https://ohmyz.sh/

首先，安装 zsh
```
sudo apt-get install zsh
```
把默认的Shell改成 zsh
```
chsh -s /bin/zsh
```

安装 oh-my-zsh:
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
更改zsh主题：
```
vi .zshrc
ZSH_THEME="ys"
```
![](1.png)

![](2.png)

### 4、 Windows Terminal 安装

在Microsoft Store里边搜索 Windows Terminal 并安装

如果希望有好用的命令行工具，微软推出的 windows terminal，支持多标签页，自定义样式，GPU 文本渲染，各种快捷键，屏幕分裂，json自定义配置等。

![](3.png)

官方网址：https://github.com/microsoft/terminal

### 5、VSCode Remote

配置WSL环境（以Ubuntu 为例）
1. 如果是国内的话最好首先更改镜像源，然后更新 Linux 分发版。某些 WSL Linux 发行版缺少 VS Code 服务器启动所需的库。 可以通过使用包管理器将其他库添加到 Linux 分发中。在Ubuntu环境下就是通过执行sudo apt-get update，更新一下包管理器。
2. 在WSL中的命令行并输入以下命令：code .（中间有空格），此步会自动布置WSL中的VSCode环境，等待片刻之后会在Windows中弹出一个VS Code窗口。在此之后也可以通过VS Code左下角的连接选项直接连接WSL。
![](4.png)

在WSL中安装C++环境
```
sudo apt-get install gcc
sudo apt-get install gdb
sudo apt-get install g++
```
在VS Code中

- 安装C/C++插件
![](5.png)

配置C/C++编译

- 创建一个C/CPP文件，按F5运行，会弹出配置C/C++，选择g++ - Build and debug active file
![](https://img-blog.csdnimg.cn/20200809193504244.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Nhb3lhbmdfSGU=,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20200809193852769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Nhb3lhbmdfSGU=,size_16,color_FFFFFF,t_70)

点击运行，在TERMINAL中查看
![](6.png)

至此，Win10 开发环境搭建完成！！！
### 其他

- [win10 wsl2修改默认安装目录到其他盘](https://blog.csdn.net/w851685279/article/details/108904106)
- [WSL2的安装详细过程](https://my.oschina.net/u/4338498/blog/4440758)
- [Windows下搭建最接近Linux体验的开发环境——使用WSL(Linux子系统)](https://segmentfault.com/a/1190000021742880)