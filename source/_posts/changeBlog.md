---
title: 换终端使用git分支更新hexo博客
date: 2019-05-11 11:03:30
categories: "Hexo教程" #文章分類目錄 可以省略
tags: [hexo,Github]
---
这些天想更新之前使用hexo搭建的博客，发现换终端之后需要重新安装环境，这里记录了如何使用git分支在换终端之后更新hexo博客。
<!--more-->

#这是换机之前的操作：先把源文件上传git分支
由于由hexo d编译部署上传到github上的不是源文件，是编译之后生成的网页：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-64a8316913f3612a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
即我们编译生成的.deploy_git里面的内容：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-d822d05bf391522a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
而我们需要的源文件目录是包括source、themes、package等文件的目录：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-97d35e5abb2366ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以我们需要把源文件目录也上传到github上，从而进行管理，首先，在你的博客Repository中新建一个“hexo”分支，名字自定：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-73fa6a0aba6818d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/15009210-d44e709587a66f26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后在setting中设置默认分支为hexo，便于推送：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-5ac791400129a7b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时候，新建目录，在目录下运行命令：
```
git clone https://github.com/huanglcat/huanglcat.github.io.git
```
把源文件分支克隆到本地，注意，克隆时只会克隆默认分支的内容，如果默认分支不是hexo，就没有克隆到源文件。

克隆下来的文件目录如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-cc7c8a5b91f5cc22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

把除了.git 文件夹外的所有文件都删掉（这些是编译后的网页文件），把之前我们写的博客源文件全部复制过来，除了.deploy_git（编译后的文件目录）。
其中，.gitignore（设置不需要git的文件或目录）文件中中需要设置部分不需要上传的目录：

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```
git不能嵌套上传，由于我之前克隆过主题，在主题中有一个.git目录：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-2bfe43b0cd582e8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
把它删掉，否则在其他终端下载时会报错。

这时候，在根目录下运行命令：
```
git add .
git commit –m "add branch"
git push 
```
查看是否正确更新：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-8533eb8f85b4e5cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#换终端的操作
git、npm、node.js等环境先安装好，设置全局用户：
```
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```
生成本机 ssh key
```
ssh-keygen -t rsa -C "youremail"
```
生成后增加在github库中。
安装hexo：
```
npm install hexo-cli -g
```
同样克隆下分支之后，安装依赖库
```
npm install
npm install hexo-deployer-git --save
```
生成和部署博客：
```
hexo g
hexo d
```
就可以更新博客了。