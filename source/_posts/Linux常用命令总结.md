---
title: Linux常用命令总结
date: 2020-07-05 22:16:17
tags: Linux
categories: Linux
---
整理一下自己在实习工作期间，常用的Linux操作命令，加深印象，熟悉一下。

### 查看日志相关：    

#### tail:  

-n  是显示行号；相当于nl命令；用例如下：

```
tail -100f test.log      实时监控100行日志

tail  -n  10  test.log   查询日志尾部最后10行的日志;

tail -n +10 test.log    查询10行之后的所有日志;
```
<!--more-->
#### head: 

跟tail是相反的，tail是看后多少行日志；例子如下：

```
head -n 10  test.log   查询日志文件中的头10行日志;

head -n -10  test.log   查询日志文件除了最后10行的其他所有日志;
```

#### cat
```
cat -n test.log |grep "debug"   查询关键字的日志
```

### vim
直接用vim查看，用 / 搜索，gg 快速跳转行首、G 快速跳转到末尾、Ctrl + F 想下翻页、Ctrl + B 向上翻页

### less / more 命令
空格翻页

#### 列出几种常见的应用场景：

应用场景一：按行号查看---过滤出关键字附近的日志
```
     1）cat -n test.log |grep "debug"  得到关键日志的行号

     2）cat -n test.log |tail -n +92|head -n 20  选择关键字所在的中间一行. 然后查看这个关键字前10行和后10行的日志:

       tail -n +92表示查询92行之后的日志

       head -n 20 则表示在前面的查询结果里再查前20条记录
```

查看日志应用场景二：根据日期查询日志
```
(1) sed -n '/2014-12-17 16:17:20/,/2014-12-17 16:17:36/p'  test.log

特别说明:

上面的两个日期必须是日志中打印出来的日志,否则无效

先 grep '2014-12-17 16:17:20' test.log 来确定日志中是否有该时间点
```

查看日志应用场景三：日志内容特别多，打印在屏幕上不方便查看，分页/保存文件查看
```
(1)使用more和less命令,

如： cat -n test.log |grep "debug" |more     这样就分页打印了,通过点击空格键翻页

(2)使用 >xxx.txt 将其保存到文件中,到时可以拉下这个文件分析

如：cat -n test.log |grep "debug"  >debug.txt
```

### 统计某一类文件的大小
```
find . -name "solar.hit.*" | xargs du -cm

du -sh solar.hit.* | awk '{sum += $1}; END{print sum}'

```

### hadoop Debug字段名是否正确
```
hadoop fs -cat  /home/mmu/txt2vid_se/index_goods/solar/raw_data/20200619/ks_mmu.video_online_all_data_daily_with_goods/000000_0 | head -n 1 | awk -F'\t' '{print $11}'

-F 是分隔符 print $11打印11列
```

### 解压缩命令

```
创建压缩
tar zcvf bin.tar.gz mr_*

解压
tar zxvf bin.tar.gz
```
### 查看文件类型

```
file test.txt
```

### 查看文件大小
```
du -sh .  查看当前文件

df -h  查看本机使用情况
```

### 显示命令

```
ls -lrth
```

### gdb 调试

```
gdb ./p4_test /data/coredump/core-1593519258-1001-1001-11-10600-p4_test

gdb bt

gdb where

```

### 查找某个字符
```

grep -r "_mm512_loadu_epi8" .  包含这个字符就显示

fgrep "_mm512_loadu_epi8" *.h  正则表达式匹配更快
```

### cut 相关

```
wc -l query.random.1k.diff | cut -d' ' -f 1  统计行数并打印 -d分隔符 -f 第一行
```

### nohup
```
nohup bash start.sh > stdout.log 2>stderr.log &  不挂断地运行命令。
```