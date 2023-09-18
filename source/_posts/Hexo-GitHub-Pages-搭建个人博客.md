---
title: Hexo + GitHub Pages 搭建个人博客
date: 2020-03-19 16:02:55
tags: Hexo
categories: Hexo
---
### 一、准备工作

#### 1、安装Node.js

(1) 下载Node.js：[官网下载地址](https://nodejs.org/en/download/) （这里以Windows 为例）;

![](/images/node.png)
<!--more-->
(2) 双击下载好的.msi文件，按下一步下一步，安装好就可以；

(3) 在cmd命令窗口，输入下面的这个命令，如果能够显示Node.js的版本，说明安装成功了。
```
node -v
```
![](/images/node_v.png)

#### 2、安装Git

(1) 下载Git：[官网下载](https://git-scm.com/downloads)

(2) 双击下载好的Git安装包，按下一步下一步，进行安装即可；

(3) 在cmd命令窗口，输入下面的这个命令，如果能够显示Git的版本，说明安装成功了。

```
git --version
```

![](/images/git_v.png)

### 二、Hexo搭建博客

#### 1、安装Hexo

（1）在计算机中，新建一个Blog文件夹，用于存放自己的博客内容。

（2）在Blog文件夹内，鼠标右键，选择Git Bash,进入到命令窗口

![](/images/blog.png)

（3）在命令窗口中，输入下面代码：

```
npm install -g hexo-cli
```

#### 2、初始化Hexo

在命令窗口中，输入下面代码:

```
hexo init
```

#### 3、配置Hexo

(1) 安装依赖

```
npm install
```

(2) 部署形成的文件

```
hexo g //或者 hexo generate
```

#### 4、启动服务器

(1) 在命令窗口执行下面代码：

```
hexo s  //或者 hexo server
```

(2) 在浏览器地址栏中输入http://localhost:4000/ （默认端口是4000），如果能够看到如图所示的效果，说明初始化的Hexo博客搭建成功了。
如图所示：

![](https://winney07.github.io/2018/08/01/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/hexo-blog.png)


#### 三、将本地的 Hexo 博客部署到 GitHub Pages上

（1）新建一个仓库，仓库名为bryceustc.github.io（这个仓库的名称必须严格按照 username.github.io 的格式来命名）【前提是你要有一个Github账号】
如图所示：（因为我已经创建过这个库了，所以会显示红色警告，只是后来为了截图，重新写一个同名的）

![](/images/github.png)

（2）进入已经创建好的仓库（点击自己的头像，选择Your profile，点击刚创建好的那个仓库进去仓库里面），点击settings，找到GitHub Pages 选项，点击 Choose a theme 选择一个主题（可以选择也可以不选择，根据自己的需求决定是否操作这一步）
如图所示：

![](/images/gitpages.png)


（3）配置Git个人信息，在winneyBlog目录下，鼠标右键，选择Git Bash，进入命令窗口，输入下面的命令

```
git config --global user.name "Github用户名"       //自己Github的账号名
git config --global user.email "Github邮箱"        //自己注册Github的邮箱地址
```

（4）生成SSH KEY,意思是生成一个公钥和密钥，因为Github需要一个密钥才能与本地相连接。在命令窗口输入下面的命令，然后需要连续按3次回车生成密钥（每按一次回车你可以看到对应的信息） 【你也可以先查看是否已经有了ssh密钥：cd ~/.ssh 如果没有密钥则不会有此文件夹，有则备份删除】

```
ssh-keygen -t rsa -C  "Github邮箱"        //自己注册Github的邮箱地址
```

如图所示：
![](https://raw.githubusercontent.com/winney07/Images/main/winney07.github.io/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/ssh-key.png)

（5）生成的SSH KEY会保存到 C:/Users/电脑名用户名/.ssh 目录中（根据你自己电脑用户名，打开对应的目录）

- 打开.ssh 这个目录，打开 id_rsa.pub 文件，复制里面的全部内容（这些内容就是密钥）
![](https://raw.githubusercontent.com/winney07/Images/main/winney07.github.io/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/add-key.png)

（6）在GitHub中添加SSH keys

- 打开Github，点击头像，选择Settings；

- 选择SSH and GPG keys项,点击右上角New SSH key按钮，将刚刚复制到的密钥粘贴到key输入框中，title自己给它命一个名就好
![](https://winney07.github.io/2018/08/01/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/add-key.png)

- 最后点击Add Key，如果显示这样的界面，说明SSH KEY 配置成功：
![](https://raw.githubusercontent.com/winney07/Images/main/winney07.github.io/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/ssh-keys.png)

（7）修改全局配置文件

- 在blog文件夹下，找到_config.yml文件；如图所示：
![](https://winney07.github.io/2018/08/01/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/deploy.png)

- 修改_config.yml文件里的deploy属性(目的是将本地hexo项目放到Github上)如图所示：
![](https://raw.githubusercontent.com/winney07/Images/main/winney07.github.io/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/deploy.png)

**注：**【如果repository中填写的是https协议的，hexo d上传代码到Github时有下面类似错误，可以将repository改为ssh的链接】
如图所示：

![](https://raw.githubusercontent.com/winney07/Images/main/winney07.github.io/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/deploy-error.png)

![](https://raw.githubusercontent.com/winney07/Images/main/winney07.github.io/Hexo-GitHub-Pages-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/git-href.png)

（8）安装hexo-deployer-git 插件，目的是将代码快速托管到Github上

```
npm install hexo-deployer-git --save
```

（9）将hexo项目托管到GitHub上（命令可以分开写也可以一起写）

```
hexo clean && hexo g && hexo d
```

备注:
hexo clean // 清除缓存文件 (db.json) 和已生成的静态文件 (public)

hexo generate // 部署之前预先生成静态文件，简写为hexo g

hexo deploy // 文件生成后立即部署网站，简写为hexo d


（10）在浏览器地址栏输入 https://username.github.io/ 即可访问，（username也就是你的Github账户名），如果能够正常访问，并且跟本地hexo项目显示的内容是一样的，那么说明你已经把本地hexo项目部署到Github上了。

#### 四、配置博客的个人信息

（1）在hexo目录中，找到全局配置文件_config.yml

（2）配置信息如下所示：

```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Bryce's Blog
subtitle: Learn Everything.
description: A Student From USTC.
author: Bryce
language: zh-Hans
timezone: Asia/Shanghai

# URL 博客地址
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://bryceustc.github.io/
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

# Directory 目录设置
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: true
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 5 #每页文章数量
  order_by: -date

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## Use post's date for updated date unless set in front-matter
use_date_for_updated: false

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
plugins: hexo-generate-feed

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/bryceustc/bryceustc.github.io
  branch: master
```

注意：.yml 文件有严格的格式要求，文件里所有的配置都是：冒号 空格 值，并且冒号是英文状态下的输入。想了解更多的可以前往官网。

（3）在博客目录下，右键点击Git Bash，进去命令窗口，输入下面的命令，即可在浏览器中看到刚刚设置的内容显示在页面中：

```
hexo server    //简写为hexo s
```
