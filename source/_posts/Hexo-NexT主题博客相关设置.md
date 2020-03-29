---
title: Hexo + NexT主题博客相关设置
date: 2020-03-21 18:00:22
tags: Hexo
categories: Hexo
---
#### 一、添加文章版权声明功能

打开博客目录下的主题配置文件（/themes/next/_config.yml），找到Declare license on posts 标签，进行配置：
```
# Declare license on posts
post_copyright:
  enable: true    #激活版权声明模块
  license: CC BY-NC-SA 3.0     #版权许可协议
  license_url: https://winney07.github.io/      #声明的文章的可点击链接（域名）
```
<!--more-->
配置完后，执行以下命令，在浏览器中访问，效果如图所示：

![](https://winney07.github.io/2018/08/02/Hexo-NexT%E4%B8%BB%E9%A2%98%E5%8D%9A%E5%AE%A2%E7%9B%B8%E5%85%B3%E8%AE%BE%E7%BD%AE/next-set1.png)

#### 二、设置favicon图标
1、选择一个favicon制作网站完成制作，例如：比特虫 ，制作一个16\*16，一个32\*32的；

2、两个不同尺寸大小的文件，重名为favicon-16x16-next.png和favicon-32x32-next.png；

3、将重命名的两个图片文件放到博客目录下的themes/next/source/images中（覆盖原来的两个默认的）
如图所示：

![](https://winney07.github.io/2018/08/02/Hexo-NexT%E4%B8%BB%E9%A2%98%E5%8D%9A%E5%AE%A2%E7%9B%B8%E5%85%B3%E8%AE%BE%E7%BD%AE/favicon.png)

### 三、添加友情链接

打开主题配置文件（/themes/next/_config.yml），找到以下内容进行修改：

```
# Blog rolls
links_icon: link
links_title: 友情链接 
# links_layout: block   //块状显示（选择了行内显示，这个要注释掉，不然会报错）
links_layout: inline    //行内显示
links:
  小超: https://www.xiaochao.me/
  Github: https://www.github.com
```

如图所示：

![](https://winney07.github.io/2018/08/02/Hexo-NexT%E4%B8%BB%E9%A2%98%E5%8D%9A%E5%AE%A2%E7%9B%B8%E5%85%B3%E8%AE%BE%E7%BD%AE/friend-link.png)

#### 四、调整hexo页面宽度

博客在浏览器上的留白太多，因此想增加文章的宽度。

打开/themes/next/source/css/_variables/custom.styl 添加两行代码即可：

```
$main-desktop = 1200px 
$content-desktop = 900px
```