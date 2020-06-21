---
title: 利用scp 远程上传下载文件/文件夹
date: 2020-06-21 23:41:15
tags: Linux
categories: Linux
---
学习工作中经常会用到scp远程下载或者传输文件/文件夹，记录一下具体的操作。

1、从服务器下载文件
```
scp username@servername:/path/filename /tmp/local_destination
```

例如：把192.168.0.101上的/home/kimi/test.txt的文件下载到 /tmp/local_destination

```
scp codinglog@192.168.0.101:/home/kimi/test.txt 
```
<!--more-->
2、上传本地文件到服务器
```
scp /path/local_filename username@servername:/path  
```

例如：把本机/var/www/目录下的test.php文件上传到192.168.0.101这台服务器上的/var/www/目录中

```
scp /var/www/test.php  codinglog@192.168.0.101:/var/www/ 
```

3、从服务器下载整个目录

```
scp -r username@servername:remote_dir/ /tmp/local_dir 
```

例如:把服务器下的test目录下载到本地的/tmp/local_dir目录
```
scp -r codinglog@192.168.0.101 /home/kimi/test  /tmp/local_dir
```

4、上传目录到服务器

```
scp  -r /tmp/local_dir username@servername:remote_dir
```

例如：把当前目录下的test目录上传到服务器 的/var/www/ 目录
```
 scp -r test      codinglog@192.168.0.101:/var/www/ 
```