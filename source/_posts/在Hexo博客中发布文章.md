---
title: 在Hexo博客中发布文章
date: 2020-03-21 17:44:15
tags: Hexo
categories: Hexo
---
#### 1、新建一篇文章

（1）在hexo博客目录下，进入Git Bash命令窗口中，输入以下命令：

```
hexo new "在这里"
```

（2）在博客目录下的/source/_posts/ 文件夹下，可以看到已经生成了标题为(在这里.md)的博客文件：
如图所示：
![](https://winney07.github.io/2018/08/02/%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E5%8F%91%E5%B8%83%E6%96%87%E7%AB%A0/zai.png)

<!--more-->
（3）在（在这里.md）文件中编辑自己的博客文章即可。
注意：Hexo 发布的文章是 Markdown 格式的文件， Markdown 基本语法的网址：[点这里前往](https://www.jianshu.com/p/191d1e21f7ed)

#### 2、给文章添加分类和标签

(1) 在（在这里.md）文件中设置tags和categories属性：
```
title: 在这里
date: 2018-08-02 11:41:10
tags:
- 博客           //多个标签可以这样添加
- hexo
categories: test
```
如图所示：

![](https://winney07.github.io/2018/08/02/%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E5%8F%91%E5%B8%83%E6%96%87%E7%AB%A0/zai-edit.png)

#### 3、启动服务器，本地测试
```
hexo s
```

#### 4、添加“阅读全文”按钮

方法一：在文章任意你想添加的位置添加即可
```
<!--more-->
```

方法二：设置首页文章以摘要形式显示，打开主题配置文件，找到auto_excerpt进行修改：
```
auto_excerpt:
  enable: true
  length: 150
```

其中length代表显示摘要的截取字符长度。

注：这两种方法，在博客首页显示的效果不一样，根据自己的需要，选择自己喜欢的方法

#### 5、在博文中添加图片

方法一：

(1)在blog目录下，安装插件：
```
npm install hexo-asset-image --save
```
(2)在blog\source 目录下新建一个images文件夹，把图片放置在里面；

(3)在xxx.md文件中引用图片：
```
![header]( images/header.jpg)
```

方法二：

(1)在全局配置文件（blog/_config.yml)中将post_asset_folder设置为true；

(2)创建文章（在创建的时候，会在blog/source/_post目录下，生成一个XXX.md文件和一个XXX的文件夹）：
```
hexo new "XXX"
```

(3)把XXX这个博文需要展示的图片放在XXX文件夹目录下；

(4)在XXX.md文件中引入图片的方式：
```
![你想输入的替代文字](XXX/图片名.jpg)
```
#### 6、发布到Github上

（1）发表的文章在本地预览无误后，在 Git Bash 命令窗口执行以下命令：

```
hexo clean && hexo g && hexo d
```

（2）在浏览器，访问自己的博客域名，即可看到刚 发布的文章