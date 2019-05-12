---
title: 记git误删的本地文件找回
date: 2019-05-12 14:07:27
categories: "Git" #文章分類目錄 可以省略
tags: [Git]
---
如何恢复git误删的本地文件？可以使用git reset命令
<!--more-->


推送本地代码到github，执行了以下两步：
```
git add .
git commit -m "update blog"
```
然后一不小心执行了下面的代码
```
git checkout hexo
```
hexo是要推送的分支，然后，辛辛苦苦写了半天的本地代码就被覆盖了！！！
不急不急，找下git历史：
执行命令：
```
git reflog
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-bf28acf01fd2cb79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
找到要恢复id，这里我要恢复到我commit的那一刻，执行命令：
```
git reset --hard 7fe0ee0
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-0cdfb891b41879f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样，代码就找回来了。