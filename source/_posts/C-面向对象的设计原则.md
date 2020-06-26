---
title: C++面向对象的设计原则
date: 2020-06-26 18:11:45
tags: C++
categories: C++
---
只知道面向对象的三个特性：封装、继承、多态。被问到面相对象的设计原则，结果答不上来，就查资料整理了一下。

1. 单一职责原则（Single Responsibility Principle）

    每一个类应该专注于做一件事情。

2. 里氏替换原则（Liskov Substitution Principle）

    超类存在的地方，子类是可以替换的。

3. 依赖倒置原则（Dependence Inversion Principle）

    实现尽量依赖抽象，不依赖具体实现。
<!--more-->
4. 接口隔离原则（Interface Segregation Principle）

    应当为客户端提供尽可能小的单独的接口，而不是提供大的总的接口。

5. 迪米特法则（Law Of Demeter）

    又叫最少知识原则，一个软件实体应当尽可能少的与其他实体发生相互作用。

6. 开闭原则（Open Close Principle）

    面向扩展开放，面向修改关闭。

7. 组合/聚合复用原则（Composite/Aggregate Reuse Principle CARP）

    尽量使用合成/聚合达到复用，尽量少用继承。原则： 一个类中有另一个类的对象。

### 参考：

1. https://blog.csdn.net/yanbober/article/details/45312243
2. https://segmentfault.com/a/1190000020182270