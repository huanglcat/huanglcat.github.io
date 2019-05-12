---
title: hexo小白学习详细教程
date: 2019-05-11 13:54:02
categories: "Hexo教程" #文章分類目錄 可以省略
tags: [hexo]
---
hexo是一个快速、简介、高效的博客框架，支持Markdown,拥有丰富的插件系统，常与GitHub等代码托管平台一起构建个人博客网站。
<!--more-->


>hexo是一个快速、简介、高效的博客框架，支持Markdown,拥有丰富的插件系统，常与GitHub等代码托管平台一起构建个人博客网站。



# 官方链接
***
[中文官网](地址：https://hexo.io/zh-cn/)

# 安装及使用
***
##### 前提：
电脑中需要已安装Git、Node.js(6.9以上)
##### 安装：
``` bash
$ npm install hexo-cli -g//安装
```
##### 建站
``` bash
$ hexo init <目录名>//初始化博客项目（最新版本已经可以在这一步安装依赖）
$ cd <目录名>//进入博客
$ npm install//安装依赖
```
命令完成后的目录如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-46b2fe43d8ba6d07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
文件夹说明：
```
|-- demo//项目跟目录名
    |-- .gitignore//git时忽略的文件或目录
    |-- package-lock.json
    |-- package.json//应用程序的信息
    |-- _config.yml//网站的配置信息
    |-- scaffolds//模板文件夹，Hexo的模板是指在新建的markdown文件中默认填充的内容。
    |   |-- draft.md
    |   |-- page.md
    |   |-- post.md//博文模板
    |-- source//资源文件夹，存放用户资源
    |   |-- _posts//博文目录
    |       |-- hello-world.md//博文
    |-- themes//主题文件夹，Hexo 会根据主题来生成静态页面
        |-- landscape.//默认主题
            ...
```


此时`package.json`中内容如下：
```
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": ""
  },
  "dependencies": {
    "hexo": "^3.8.0",
    "hexo-generator-archive": "^0.1.5",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-index": "^0.2.1",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.1",
    "hexo-renderer-stylus": "^0.3.3",
    "hexo-renderer-marked": "^0.3.2",
    "hexo-server": "^0.3.3"
  }
}

```
##### 修改配置：
配置修改教程：[配置](地址:https://hexo.io/zh-cn/docs/configuration)
修改配置文件中必须修改的几项，其余可根据配置自行修改：
* url：网站地址，必须修改，此处博文是托管在github上，故此使用http://youname.github.io格式作为网站名字
* language：语言，设置中文，根据需要修改，中文为`zh-CN`
注意：配置值与配置名需要隔一个空格，否则会编译报错
```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Hexo小白的博客
subtitle:
description:
keywords:
author: Hexo小白
language: zh-CN
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://emmaHuang.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
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
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:
  
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date
  
# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type:

```
##### 运行
``` bash
$ hexo server//运行本地服务
```
在浏览器打开“http://localhost:4000/”
![image.png](https://upload-images.jianshu.io/upload_images/15009210-2df45e828128018c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到小站已经建起来了。
##### 使用
用hexo进行写博文的命令使用如下：
1. 新建博文
```
hexo new "my first blog"//有逗号必须使用引号括起来
```
在`source/_posts/`下生成文件`my-first-blog.md`如下：
```
---
title: my first blog
date: 2019-05-11 16:20:56
tags:
---
```
这里使用`---`分割的区域叫做“Front-matter”，用于指定这篇博文的变量
此时如果运行了`hexo server`,刷新浏览器时可看到新建的博文：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-0df9edcc39520440.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可手动修改`Front-matter`：
```
---
layout:
title: my first blog
date: 2019-05-11 16:20:56
updated:
comments:
tags:
- introduction
- hexo
categories:
- Diary
---

```
tags表示标签， categories表示分类，修改之后刷新如下:
![image.png](https://upload-images.jianshu.io/upload_images/15009210-c25e60bfdbcb4d04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 生成静态文件
```
hexo generate//简写hexo g 
hexo g -d//文件生成后立即部署网站
hexo g -w//监视文件变动
```
3. 发表草稿
```
hexo publish "my first blog"
```
4. 启动本地服务器
```
hexo server [-p 4001] //可以修改端口
```
5. 部署网站
```
hexo deploy
hexo d//简写
hexo d -g//部署之前先生成静态文件
```
6. 清除缓存(db.json)和已经生成的静态文件(public)，当发现对站点的更改无效时，比如更换主题后，执行此命令
```
hexo clean
```
7. 列出网站资料
```
hexo list
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-a25e2362c2a7a383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 部署
hexo提供一键部署的功能，命令：
```
hexo deploy//简写hexo d
```
在开始之前，需要安装deployer和在`_config.yml`中进行配置：
安装deployer(这里只记录Git方式)
```
npm install hexo-deployer-git --save

```
修改配置
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy: 
  type: git
  repo: git@github.com:emmaHuang1992/emmaHuang1992.github.io.git
  branch: public
  message: publish blog
```
生成站点文件并推送远程库：
```
hexo clean//清除站点文件
hexo deploy//重新生成站点文件并推送
```
推送之前，在库设置（Repository Settings）中将默认分支设置为_config.yml配置中的分支名称。稍等片刻，站点就会显示Github Pages中

可设置两个分支（根据自己的习惯自行配置）：
* master：存放源代码
* public：存放编译部署后的站点文件

开始在github中新建代码仓库：
1. 新建的repository名字要与账号对应，格式：youname.github.io
2. 生成本机ssh
```
ssh-keygen -t rsa -C "email@xx.com"
```
在目标目录中找到`id_rsa.pub`打开
![image.png](https://upload-images.jianshu.io/upload_images/15009210-a54cc41b286702d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
复制内容到github对应库中的settings->Deploy keys->Add new->复制粘贴公钥->选中确认写入权限->添加
![image.png](https://upload-images.jianshu.io/upload_images/15009210-ba6e777fef72735f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后执行以下命令：

``` bash
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_rsa
$ clip < ~/.ssh/id_rsa.pub
$ ssh -T git@github.com//测试下公钥有没有添加成功
```


# 主题配置
***
如果不想使用默认的主题，也可以自己下载一个新的主题，放在themes目录下，并修改 _config.yml 内的 theme 设定，即可切换主题。
拿使用广泛的next主题为例：[next使用教程](http://theme-next.iissnan.com/getting-started.html)
1. 在themes目录下克隆next主题
```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
2. 切换主题
```
## Themes: https://hexo.io/themes/
theme: next
```
这时候，重新hexo clean,hexo g，hexo s，就可以看到主题更新啦！

