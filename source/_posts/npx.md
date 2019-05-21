---
title: npx详细使用
date: 2019-05-18 11:55:13
categories: "npm" #文章分類目錄 可以省略
tags: [npm,npx]
---
前几天接触到了npx，发现这个命令非常好用，这里总结下npx的使用。
<!--more-->




# npx是什么？
***
npx是npm5.2之后发布的一个命令。官网说它是“execute npm package binaries”，就是执行npm依赖包的二进制文件，简而言之，就是我们可以使用npx来执行各种命令。
npx官网：https://www.npmjs.com/package/npx

# 为什么要使用npx？
***
## 解决的问题
### 在命令行执行本地已安装的依赖包命令
>__使用npx可以在命令行直接执行本地已安装的依赖包命令，不用在scripts脚本写入命令__，也不用麻烦的去找本地脚本。

首先来看这个场景：
我们本地安装了一个依赖包：
```
npm i -D mocha
```
想要在本地（当前目录）执行它时，什么都不做时是不能运行这个命令的：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-3b360d8c00b6d9db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们一般会使用几种方式来运行我们想要运行的命令：
1. 使用`package.json`的scripts脚本
```
//package.json
"scripts": {
  "findmocha": "mocha --version",
}
```
然后在命令行运行：
```
npm run findmocha
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-352a9b70d2c491f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 在命令行中直接找到模块的二进制文件运行
3. 全局安装模块

而使用npx，我们可以直接在命令行执行我们要运行的命令：
```
npm i -D mocha
npx mocha --version
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-0c46415adf29b0d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 不用全局安装，直接在命令行执行一次性命令
>有很多命令，我们只需要执行一次的，但是却要全局安装一次，实在不科学，使用npx，可以在不全局安装依赖包的情况下，运行命令，而且运行后不会污染全局环境

比如
```
npx create-react-app my-react-app
```
npx 将create-react-app下载到一个临时目录，使用以后再删除。
每次运行这个命令，都会重新下载依赖包，运行后删除。
### 切换node版本来运行命令
>当你想要运行的命令不兼容当前的nodejs版本，可以通过npx来切换版本，指定某个版本的 Node 来运行命令。

npx的-p选项指定要安装的包，并将其添加到正在运行的$PATH中
如：
```
 npx node@6 -v
 npx node@7 -v
 npx node@8 -v
```
以上的命令，会自动下载需要的node，执行完命令后删除。

## npx的原理
npx的原理，就是在运行它时，执行下列流程：
1. 去`node_modules/.bin`路径检查npx后的命令是否存在，找到之后执行；
2. 找不到，就去环境变量`$PATH`里面，检查npx后的命令是否存在，找到之后执行;
3. 还是找不到，自动下载一个临时的依赖包最新版本在一个临时目录，然后再运行命令，运行完之后删除，不污染全局环境。

# 安装和参数说明
***
## 安装
```
npm install -g npx
```
## 常用参数
###  -p 参数
-p参数用于指定 npx 所要安装的模块
```
npx -p node@6 node -v
```
### --no-install 参数
强制使用本地模块，不下载远程模块，如果本地不存在该模块，就会报错。
### --ignore-existing 参数
忽略本地的同名模块，强制安装使用远程模块
# 使用场景总结
## 使用npx执行 本地命令
```
npm i -D mocha
npx mocha --version
```
## 使用npx一次性执行命令
```
npx create-react-app my-react-app
```
## 使用npx切换node版本
```
 npx node@6 -v
```
## 使用npx执行 GitHub 源码
```
npx github:piuccio/cowsay
```
远程代码必须是一个模块，即必须包含package.json和入口脚本
## 使用npx开启一个静态服务器
```
npx http-server    #默认返回根目录下index.html
npx http-server -p 3000  #指定端口
```




参考链接：
https://www.npmjs.com/package/npx
http://www.ruanyifeng.com/blog/2019/02/npx.html
https://www.jianshu.com/p/a4d2d14f4c0e
