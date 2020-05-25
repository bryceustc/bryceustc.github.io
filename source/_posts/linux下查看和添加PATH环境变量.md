---
title: linux下查看和添加PATH环境变量
date: 2020-05-23 00:39:20
tags: Linux
categories: Linux
---
$PATH：决定了shell将到哪些目录中寻找命令或程序，PATH的值是一系列目录，当您运行一个程序时，Linux在这些目录下进行搜寻编译链接。

编辑你的 PATH 声明，其格式为：
```
PATH=$PATH:<PATH 1>:<PATH 2>:<PATH 3>:------:<PATH N>
```
你可以自己加上指定的路径，中间用冒号隔开。环境变量更改后，在用户下次登陆时生效，如果想立刻生效，则可执行下面的语句：$ source .bash_profile
<!--more-->
#### 可用 export 命令查看PATH值
```
(base) bryce@ubuntu:~$ export 

declare -x CFLAGS="-I/usr/local/cuda-10.0/include "
declare -x CLUTTER_IM_MODULE="xim"
declare -x COLORTERM="truecolor"
declare -x CONDA_DEFAULT_ENV="base"
declare -x CONDA_EXE="/home/bryce/anaconda3/bin/conda"
declare -x CONDA_PREFIX="/home/bryce/anaconda3"
declare -x CONDA_PROMPT_MODIFIER="(base) "
declare -x CONDA_PYTHON_EXE="/home/bryce/anaconda3/bin/python"
declare -x CONDA_SHLVL="1"
declare -x CPATH="/var/nccl-repo-2.5.6-ga-cuda10.0/include:"
declare -x CUDA_HOME="/usr/local/cuda-10.0"
declare -x CUDA_PATH="/usr/local/cuda-10.0"
declare -x DBUS_SESSION_BUS_ADDRESS="unix:path=/run/user/1000/bus"
declare -x DEFAULTS_PATH="/usr/share/gconf/ubuntu.default.path"
```

#### 单独查看PATH环境变量

```
(base) bryce@ubuntu:~$ echo $PATH 

/home/bryce/anaconda3/bin:/home/bryce/anaconda3/bin:/home/bryce/.local/share/umake/bin:/home/bryce/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/local/cuda-10.0/bin:/opt/google/chrome
```

#### 添加PATH环境变量

1.命令行
```
[root@localhost u-boot-sh4]$ echo $PATH

/usr/kerberos/sbin:/usr/kerberos/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```
添加PATH环境变量，可用：

```
[root@localhost u-boot-sh4]$ export PATH=/opt/STM/STLinux-2.3/devkit/sh4/bin:$PATH
```
再次查看：
```
[root@localhost u-boot-sh4]$ echo $PATH
/opt/STM/STLinux-2.3/devkit/sh4/bin:/usr/kerberos/sbin:/usr/kerberos/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```
说明添加PATH成功。

第二种方法：
```
$ vim /etc/profile
```
在文档最后，添加:
```
export PATH="/opt/STM/STLinux-2.3/devkit/sh4/bin:$PATH"
```
保存退出(:wq)，然后运行：
```
$ source /etc/profile
```
不报错则成功。