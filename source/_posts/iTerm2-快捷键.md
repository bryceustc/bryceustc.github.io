---
title: iTerm2 快捷键
date: 2020-05-24 14:48:25
tags: Linux
categories: Linux
---
总结了工作过程中常用到的iTerm的快捷键

### 光标控制
```
ctrl + a: 到行首
ctrl + e: 行末
ctrl + f/b: 前进后退，相当于左右方向键，但是显然比移开手按方向键更快
ctrl + p: 上一条命令，相当于方向键上
ctrl + r: 搜索命令历史，这个大家都应该很熟悉了
ctrl + d: 删除当前字符
ctrl + h: 删除之前的字符
ctrl + w: 删除光标前的单词
ctrl + k: 删除到文本末尾
ctrl + t: 交换光标处文本
⌘ + —/+/0: 调整字体大小
⌘ + r:清屏，其实是滚到新的一屏，并没有清空。ctrl + l 也可以做到。
```
<!--more-->
### 标签页操作
```
垂直分屏：command + d

水平分屏：command + shift + d

切换屏幕：command + option + 方向键 command + [ 或 command + ]

查看历史命令：command + ;

查看剪贴板历史：command + shift + h
```
### 窗口操作
```
垂直分屏：command + d

水平分屏：command + shift + d

切换屏幕：command + option + 方向键 command + [ 或 command + ]

查看历史命令：command + ;

查看剪贴板历史：command + shift + h
```

### 选中即复制
iTerm2 有 2 种好用的选中即复制模式。
- 一种是用鼠标，在 iterm2 中，选中某个路径或者某个词汇，那么，iterm2 就自动复制了。 　　
- 另一种是无鼠标模式，command+f,弹出 iterm2 的查找模式，输入要查找并复制的内容的前几个字母，确认找到的是自己的内容之后，输入 tab，查找窗口将自动变化内容，并将其复制。如果输入的是 shift+tab，则自动将查找内容的左边选中并复制。

### 自动完成

- 输入打头几个字母，然后输入 command+; iterm2 将自动列出之前输入过的类似命令。 

### 剪切历史
输入 command+shift+h，iterm2 将自动列出剪切板的历史记录。如果需要将剪切板的历史记录保存到磁盘，在 Preferences > General > Save copy/paste history to disk 中设置。