---
title: vue-cli 3.0从零快速创建vue项目原型框架
date: 2019-05-12 10:59:35
categories: "Vue" #文章分類目錄 可以省略
tags: [Vue]
---
Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统。下面记录了如何使用Vue CLI 3从零快速创建vue项目原型框架。
<!--more-->


# 环境配置：
本机操作系统：window7

安装`node.js`和`npm`：`node.js`(`vue-cli 3.0`安装需要nodejs版本大于`8.9.0`，本机安装的是`11.6.0`)

查看本机node.js版本：
```
node -v
```

如果node.js版本低，可以使用一个windows下nodejs的版本管理工具gnvm来更新nodejs版本，可以更新到最新版本，也可以更新到特定版本（文档地址：http://ksria.com/gnvm/），在本机存在nodejs环境时，下载解压缩获取到gnvm.eve后将其保存到nodejs的安装目录下，执行命令更新到最新版本：
```
gnvm update latest
```
本机npm版本6.5.0，查看npm版本命令：
```
npm -v
```
安装`@vue/cli` + `@vue/cli-service-global`：`vue-cli3`是vue更新的构建工具，降低了使用webpack的难度，支持热更新，有`webpack-dev-server`支持，搭建了一个测试服务器。（文档地址：https://cli.vuejs.org/zh/guide/）通过 @vue/cli + @vue/cli-service-global 可以快速开始零配置原型开发。

安装命令：
```
npm install -g @vue-cli

npm install -g @vue/cli-service-global
```
如果已将安装了vue-cli旧版本，需要先卸载就版本，再安装新版本。（官网有说明）
安装开发工具Visual Studio Code：Visual Studio Code (简称 VS Code / VSC) 是一款免费开源的现代化轻量级代码编辑器，支持几乎所有主流的开发语言的语法高亮、智能代码补全、自定义快捷键、括号匹配和颜色区分、代码片段、代码对比 Diff、GIT命令 等特性，支持插件扩展，并针对网页开发和云端应用开发做了优化。软件跨平台支持 Win、Mac 以及 Linux。内置Git终端。

下载地址：https://code.visualstudio.com/

安装成功之后，最好安装一些方便vue开发的插件，在扩展中输入vue，可查询vue相关插件，Vue VSCode Snippets可以提供快速构建代码块命令，安装即可使用快捷命令。


此时，环境已准备好。

# 创建新项目
可以根据使用习惯选择命令行或者UI界面创建新项目：

## 命令行快速创建新项目：
在vscode的下面打开终端，输入创建项目命令：
```
vue create vue-new
```

![image.png](https://upload-images.jianshu.io/upload_images/15009210-cb16859221a4e3dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以使用默认配置（只配置babel 和 eslint）也可以使用自定义配置，通过键盘上下键可切换选择，此时选择自定义配置Manually select features 后按空格键选中/反选自定义配置，按a键 全选/全不选：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-87c9281e0548fd62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

仍然是通过键盘上下键选择你要的配置，再通过空格键选择配置，配置说明如下：

* Bacel：配置Bacel（配置Bacel可以自由的在开发环境使用es6语法，Bacel会将你的es6语句编译为es5语句）

* TypeScript：配置TypeScript开发环境

* Progressive Web App (PWA) Support：配置对PWA的支持（PWA全称Progressive Web App，直译是渐进式WEB应用，是 Google 在 2015 年提出，2016年6月才推广的项目）

* Router：配置vue router

* Vuex：配置Vuex

* CSS Pre-processors：配置css预处理类型

* Linter / Formatter：配置Linter / Formatter规范类型

* Unit Testing：配置单元测试方式

* E2E Testing：配置E2E测试方式

这里选择的自定义配置如下：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-cb9d683ff7f18243.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择好回车之后，根据提示输入Y或N ，或者直接回车选择默认配置：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-b2700d9775048458.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选好配置回车之后，会提示是否保存为之后创建新工程的默认配置：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-5fa76d266acc22d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里直接回车确认，等待命令完成


![image.png](https://upload-images.jianshu.io/upload_images/15009210-b56dc99fde4716aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时已经创建好一个新项目。

## 使用图形化界面创建新项目
如果不喜欢在命令行进行操作，也可以在图形化界面创建新项目，在vscode中打开终端，输入命令：
```
vue ui
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-15095dcd996cc3f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到该命令创建了一个本地服务，打开了一个浏览器，窗口如下：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-49b7fe9efb93b6b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击创建新项目：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-f37d177eb8857961.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入项目名，选择包管理器（npm）之后

![image.png](https://upload-images.jianshu.io/upload_images/15009210-b5cc46b86f1fb9ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击下一步，可以看到，同命令行一样的，进行项目配置：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-3f5b2d47c835798b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择手动配置，点击下一步：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-59a0ebd1a738cfce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择自定义配置后点击下一步：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-83a68b703f2e47df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择具体配置后，点击创建新项目：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-fb3a94bf22649ff4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
保存配置之后，点击创建，看到loading界面，安装成功后，可以看到一个管理页面：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-2b1281d6b5207aee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以在上面管理插件：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-d910b19420ca1acf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
配置依赖：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-c0bc8e181455e3c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
修改配置：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-8d1cca5245e42c2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行和管理任务：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-3994f834fce8249e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点击运行serve命令，可以看到项目的各项配置，资源和依赖项

![image.png]("https://upload-images.jianshu.io/upload_images/15009210-ad8a997d3e2cbc70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击启动app，可以看到运行起来的页面：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-85da0162f17d13e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
还可以分析项目的打包情况：


![image.png](https://upload-images.jianshu.io/upload_images/15009210-2426598e081fdfc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时，创建项目成功。

# 添加UI库插件
如果没有特殊的业务需求或者设计展现，可以根据业务特点，选一款UI库，例如如果是移动端，可以选择滴滴团队的UI框架cube-ui，

输入命令：
```
vue add cube-ui
```

# 项目运行
此时可以开始进行开发了。打开项目目录，可以看到项目目录如下：

![image.png](https://upload-images.jianshu.io/upload_images/15009210-754be52952585ffd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入命令运行项目：
```
npm run serve
```

可以将此命令改成start，


执行start命令不用输入run：
```
npm start
```

![image.png](https://upload-images.jianshu.io/upload_images/15009210-9b9ddbd656194fcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时，已经创建好一个vue项目原型框架，可以尽情地开始开发啦！


