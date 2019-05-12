---
title: vue-cli3项目本地mock数据时报404错误
date: 2019-05-12 15:13:24
categories: "Vue" #文章分類目錄 可以省略
tags: [Vue,vue-cli3]
---
vue-cli3下创建的项目,在vue.config.js中的devServer开发服务器中的前置中间件mock数据，运行报错的处理。
<!--more-->

环境：

window7、node.js(11.6.0)

项目：

vue-cli3下创建的项目，已安装axios，使用自带的webpack-dev-server来mock数据：

在根目录下创建vue.config.js扩展webpack设置：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-6fb868efe2ca4236.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在vue组件中获取数据：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-ac8645b8e68aa484.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


查看页面发现报404错误，注意两点：

1. mock配置文件中修改之后需要重启服务，否则不会更新；

2. 如果启动服务过程中检测到代码有错误，但是服务仍然启动成功的，需要解决错误直至错误完全解决，否则会影响配置更新。

在命令行中使用ctrl+C终止服务，重新启动npm start 即可。