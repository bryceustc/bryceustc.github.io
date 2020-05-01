---
title: 学习Git总结
date: 2020-05-01 17:40:52
tags: Git
categories: 工具
---
针对学习Git的操作命令进行了一个总结。具体如下面这张思维导图，记录一下提纲，方便自己建立完整的Git知识体系。

![](1.png)

<!--more-->

## 一、 原理

### 1. 三个工作区

![](2.jpg)

![](3.jpg)

### 2. 常用命令

![](4.jpg)

## 二、 操作

### 1. 设置

- git init

    ![](5.jpg)

    - 在LearnGit目录下创建一个版本库该目录下的所有内容都会被作为版本库的组成，会默认创建一个隐藏的.git目录，用来存放版本库的全部原数据。

- git config

    ```
    git config --global user.name "bryceustc"
    git config --global user.mail bryce@mail.ustc.edu.cn
    ```
    用来对本地版本库进行一些设置  比如本地版本库的拥有者是谁 联系方式是怎样的
    也就是这两个参数 :user.name和user.email 
    1. 仅这两个全局变量值是必须设置的。但所有可设置的值超过130个。其中大部分都很少用得上。
    2. 命令中global的意思是 把这个设置应用于本地计算机的所有地方，也就是说你在本地所有地方建立的版本库拥有者和联系方式都是这个

    ![](6.jpg)

    可以用命令``git config --global --list ``来看一下上面设置的一些属性

### 2. 编辑

- git add

    - 这个命令的功能是将文件添加到暂存区
    
        可以有两种方式去去使用

        一种是通过直接git add 的方式把要添加的文件跟在后面 也就是通过命令行的方式

        一种是通过git add -i的方式调出交互式接口来使用 更多子命令功能更丰富
        这里只说第一种方式的使用
    
    ![](7.jpg)

    - 在目录下建立文件README.md 内容如上

    ![](8.jpg)

    - 输入 git add README.md 即可。如果没有任何提示说明已经添加成功了

- git commit

	- 把修改后的文件从本地暂存区 提交到 本地仓库 (注意不是远程仓库)
使用git commit 提交缓冲区文件到本地仓库的时候必须写好备注 也就是说明提交的原因 也就是得加上-m参数  像下面这样 
这样默认会把暂存区所有的文件都推到本地仓库
        ```
        git commit-m" first commit"
        ```
        ![](9.jpg)

	- 如果只是想把暂存区部分文件推到本地仓库可以在后面加上你要推的指定的文件名
        ```
        git commit -m"first commit "  test3.txt
        ```
	-  git commit -C HEAD -a --amend这个命令的功能是在想改进上次提交里面的某些错误

        -C 参数表示就采用和上次提交一样的备注

        HEAD参数表示提交名称

        -a表示提交所有暂存区的文件
        
        --amend就是表示把改动融合进上次的提交里面

- git status

	- 没有git add

    ![](11.jpg)

	- git add 以后

    ![](10.jpg)

	- git commit 以后

    ![](12.jpg)

	- 删除工作区文件之后

    ![](14.jpg)

	- 改动工作区文件以后

    ![](13.jpg)
    ![](15.jpg)

- git diff

	- 改动工作区文件以后

    ![](16.jpg)

	- git diff 不加参数 

        （1）当暂存区中没有文件时，git diff比较的是，工作区中的文件与上次提交到版本库中的文件。
        
        （2）当暂存区中有文件时，git diff则比较的是，当前工作区中的文件与暂存区中的文件。
	- git  diff --cached 比较暂存区和本地仓库文件的区别
	- git diff HEAD 比较的是工作区中的文件与版本库中文件的差异(当前分支上)  
	- git diff new 将当前分支和指定分支new对比

- git mv 

	- git mv  把工作区的文件重命名或者移动 

    比如
    ```
    git  mv  README.md 
    git mv readme.md
    ```
	- 你也可以不用git mv 

        直接在操作系统下鼠标操作移动或者重命名但是这样的话你得重新git add 然后git commit 而用了git mv 的话只要git commit就可以了 

- git rm

	- git rm  把工作区的文件删除
        比如
        ```
        git  rm  readme.md
        ```
	- 你也可以不用git rm

        直接在操作系统下鼠标操作删除但是这样的话你得重新git add 然后git commit 以后还要把原来版本库中对应的那个文件删除掉git rm 而用了git mv 的话只要git commit就可以了 

### 3. 日志

- git log

    - git log 47f9e8e 传递一个指定的版本，并以此作为查看日志的起始点（查看从那时起到现在的全部提交）

    - git log --since="5 hours"查看最近5小时内的提交

        ![](17.jpg)

        git log 可以显示提交的日志项，包括是谁，何时做的提交，以及本次提交的具体代码改动（可选的）等信息 按照时间倒序显示 后面可以通过设置各种各样的参数来过滤日志

        每个提交有四部分信息：提交名称、提交人、提交日期、提交留言

    - git log --before="5 hours" -1来查看5小时之前的最后一个

    - git log 47f9e8e..440b963用“最老版本..最新版本”这种格式作为查找范围。最老版本在前，最新版本在后

- git reset

    git reset --hard HEAD  回退到head之前的那一个版本

    git reset --hard HEAD^  回退到head之前在之前的一个版本

    git reset --hard 版本库ID  回退到版本库ID的那个版本

    ![](18.jpg)

    ![](19.jpg)

    ![](20.jpg)

    ![](21.jpg)

    Git reset 命令有三个主要选项：git reset --soft; git reset --mixed; git reset --hard;
    ```
    git reset --soft
    ```
    将HEAD引用指向给定提交。索引（暂存区）和工作目录的内容是不变的，在三个命令中对现有版本库状态改动最小。
    ```
    git reset --mixed（git reset默认的模式）
    ```
    HEAD引用指向给定提交，并且索引（暂存区）内容也跟着改变，工作目录内容不变。这个命令会将索引（暂存区）变成你刚刚暂存该提交全部变化时的状态，会显示工作目录中有什么修改。
    ```
    git reset --hard
    ```
    HEAD引用指向给定提交，索引（暂存区）内容和工作目录内容都会变给定提交时的状态。也就是在给定提交后所修改的内容都会丢失(新文件会被删除，不在工作目录中的文件恢复，未清除回收站的前提)。

    ![](22.jpg)

- git revert

![](23.jpg)

![](24.png)

![](25.png)

![](26.jpg)


### 4. 分支

- 分支的介绍

    在项目开发的过程中 有时需要添加新功能 测试新算法  修正新bug 如果只有一条版本演进的轨迹 则很难实现，故需要引入多条分支来开发

-  git branch

    - git branch -m master mymaster 分支重命名

    - git branch 查看当前分支

    - git branch mybranch 创建一个新分支 mybranch

    - git branch -r 显示远程分支

    - git branch -d mybranch 删除分支mybranch 只有当要删除的分支
    
        已经成功合并到当前分支时删除才会成功  可以把小写的d改成大写的D这样就不会检查 强行删除
- git checkout
    
    - git checkout mybranch   切换到新分支 mybranch

    - git checkout -b mybranch  创建并切换到新分支mybranch

    - git checkout -b mybranch master   基于主分支上创建并切换到新分支mybranch

- git merge

    - git merge mybranch  把分支mybranch合并到当前分支上 
    
        当mybranch添加内容会添加到当前分支上  在mybranch中删除的内容在当前分支也会删除 但是当两个分支在同一个文件的同一行发生差异时 合并时就会发生冲突 从而也就不能自动合并得人工处理

        分支冲突处理:Git不能自动合并时，称之为冲突(conflict）。冲突总是发生在对不同分支上的同一文件的同一文本块以不同的方式修改，并试图合并的时候,它就会停下来等待人工处理。
        比如在master 分支下commit了一个readme.md文件 
        在new1分支下也commit了一个readme.md文件 
        这两个文件在第2行是不同的
        这个时候你跳到master分支上用git merge new 就会发生冲突
        而且在本地生成一个需要你修改的临时文件 
        你需要对他进行修改
        改成你觉得合适的结果
        然后git add ,git commit 就完成了合并