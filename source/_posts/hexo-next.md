---
title: Next主题个性化配置
date: 2019-05-11 21:03:35
categories: "Hexo教程" #文章分類目錄 可以省略
tags: [hexo]
---
Next是hexo中众多主题之一，在进行hexo博客中各种配置时遇到了不少坑，这里一一记录一下。
<!--more-->


## 修改布局风格
***
Next默认风格是Muse，可修改为其他，在`themes/next/_config.yml`中修改配置
```
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini
```
## 修改菜单目录
***
Next主题默认只有主页和和关于，如果要增加菜单，在`themes/next/_config.yml`中修改配置，
```
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags #标签
  categories: /categories/ || th #分类
  archives: /archives/ || archive #归档
  guestbook: /guestbook/ || comment #留言
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat

# Enable/Disable menu icons.
menu_icons:
  enable: true
```
注意，留言页默认是没有的，需要自己增加：
```
hexo new "guestbook"
```
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-3fc726e3e5451d9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 添加头像图片
***
在`themes/next/_config.yml`中修改配置，
```
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.gif
# in site  directory(source/uploads): /uploads/avatar.gif
avatar: /uploads/avatar.jpg
```
图片需要存在目录路径
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-4ad01124d341c04d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 添加社交账号
***

在`themes/next/_config.yml`中修改配置，
```

# ---------------------------------------------------------------
# Sidebar Settings
# ---------------------------------------------------------------

# Social Links.
# Usage: `Key: permalink || icon`
# Key is the link label showing to end users.
# Value before `||` delimeter is the target permalink.
social:
  GitHub: https://github.com/yourname || github
  E-Mail: mailto:yourname@gmail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

social_icons:
  enable: true
  icons_only: false
  transition: false
```
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-ead46140dda8d6f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 添加友情链接
***

在`themes/next/_config.yml`中修改配置，
```

# Blog rolls
links_icon: link
links_title: Links
links_layout: block
#links_layout: inline
links:
  简书偶余杭: https://www.jianshu.com/u/8af7f2837baf
```
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-205c446fae6d56e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 添加背景动画
***

在`themes/next/_config.yml`中修改配置，这里选了`canvas_next`，有四种效果，根据自己的喜好选择。（如设置了没有成功，请【1.更新Next主题版本;2.运行命令：hexo clean；hexo g；hexo d】）
```
# Canvas-nest
canvas_nest: true

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false
```
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-10f109487ab28b0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 文章字数统计
***
安装插件：

```
npm i --save hexo-wordcount
```

在`themes/next/_config.yml`中修改配置，

```
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
  totalcount: true
  separated_meta: true
```
打开 post.swig 文件，`/themes/next/layout/_macro/post.swig`，在对应数字后增加单位：
字数：
```
<span title="{{ __('post.wordcount') }}">
    {{ wordcount(post.content) }} 字
</span>
```
阅读时长：
```
<span title="{{ __('post.min2read') }}">
    {{ min2read(post.content) }} 分钟
</span>
```
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-6d247f5afbb80a7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 增加站内搜索
***
安装插件：
```
npm install hexo-generator-search --save
```
在`themes/next/_config.yml`中修改配置：
```

# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
```
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-410ac5730e988c4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/15009210-7277c59088f68031.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 添加版权声明
***
在`themes/next/_config.yml`中修改配置：

```
# Declare license on posts
post_copyright:
  enable: true
  license: CC BY-NC-SA 3.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/3.0/
```
效果：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-a2cc796cd9b3bce4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 配置页面访问量
***
使用LeanCloud作为服务后台，
先注册一个LeanCloud账号：
[Leancloud官网](https://leancloud.cn/)
创建一个应用，名字随便
在应用中的存储中创建一个Class，名字叫做`Counter`,  权限设置为无限制:
![image.png](https://upload-images.jianshu.io/upload_images/15009210-b85a2bc4374638e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在`themes/next/_config.yml`中修改配置：
```

# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: 
  app_key: 
  security: false
  betterPerformance: false

```
其中 app_id 和 app_key 在 LeanCloud 的设置 -> 应用 Key 可以找到
效果如下:
![image.png](https://upload-images.jianshu.io/upload_images/15009210-034cfa837be4dd5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
记录文章访问量的唯一标识符是文章的发布日期以及文章的标题，因此请确保这两个数值组合的唯一性，如果你更改了这两个数值，会造成文章阅读数值的清零重计
## 增加评论功能
***
使用valine+LeanCloud增加评论功能。
在`themes/next/_config.yml`中修改配置：
```

# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
valine:
  enable: true
  appid: 
  appkey: 
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: 说点什么吧！ # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size

```
注意appid和appkey的名字需要和项目中的valine.swig文件中的配置一致。否则不会加载评论框的哦。
这里需要注意，为了安全和valine运行正常，需要在LeanCloud中增加安全域名：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-d33e40c8b4326f00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
效果如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-c5cb988b77f59d26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是我们又不想所有的页面都有评论，这时候，可以在页面的`Front-matter`中增加`comments: false`,就可以不显示评论：
```
---
title: 关于
date: 2018-03-24 22:24:28
type: "about"
comments: false
---

```