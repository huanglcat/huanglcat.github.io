---
title: npm基础知识和命令总结
date: 2019-05-15 09:34:46
categories: "npm" #文章分類目錄 可以省略
tags: [npm]
---
npm是工作过程中很常用的包管理工具，这里记录汇总npm基础知识和命令。
<!--more-->


# npm是什么？
***
npm全称Node Package Manager，是node.js的模块依赖管理工具，它有一个日益强大的对手叫yarn，yarn是Facebook发布的一款依赖管理工具。
## npm的使用场景：
上传分享自己写的程序代码（包），下载别人写的程序代码（包）。
## npm的组成：
* npm官网（https://www.npmjs.com/），用来管理设置上面的代码程序包
* 一个大数据库，大家分享的程序就放在那里
* 命令行工具（CLI），我们通过CLI来与npm交流
# npm怎么安装与升级
***
与NodeJs一起集成安装，安装NodeJS时安装npm。
## 查看npm版本
```
npm -v
#6.5.0
```
## 查看帮助
```
npm help <command>
```
## 升级npm
```
npm install npm -g
npm install npm@latest -g #升级到最新版本
```

# 包是什么？
***
npm的核心是包，npm将它管理的程序都叫包，每个包里有个`package.json`文件，位于包的根目录下，用于定义包的属性（配置信息），比如包的名称、版本、许可证等等。
在进行`npm install`命令时，就是根据这个配置文件，来自动下载这个包所需的模块，配置项目所需的运行和开发环境。
## package.json
`package.json`是一个JSON对象，每一个键值对就是当面包的一个配置。
一个`package.json`常用字段:
```
{
	"name": "Hello World", //包名
	"version": "0.0.1",//包的版本号，主版本.次版本.补丁版本
	"author": "张三",//包的作者，格式设置：Your Name <email@example.com> (http://example.com)
	"description": "第一个node.js程序",//包的描述
	"keywords":["node.js","javascript"],//包的关键词
        "main":"index.js",//main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
	"repository": {// 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
		"type": "git",
		"url": "https://path/to/url"
	},
	"license":"MIT",//包的版权协议
	"engines": {"node": "0.10.x"},//该模块运行的平台，比如 Node 的某个版本或者浏览器
	"bugs":{"url":"http://path/to/bug","email":"bug@example.com"},
	"contributors":[{"name":"李四","email":"lisi@example.com"}],//包的其他贡献者姓名
	"scripts": {//运行脚本命令的npm命令行缩写，执行命令：npm run <命令名>
		"start": "node index.js"
	},
        "config":{//添加命令行的环境变量
                "port":"8080"//可以在js中通过process.env.npm_package_config_port获取，可以通过npm config set <包名>:port 80修改
        },
        "browser": {//供浏览器使用的版本
                "tipso": "./node_modules/tipso/src/tipso.js"
        },
	"dependencies": {//项目运行依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 * node_module 目录下
		"express": "latest",
		"mongoose": "~3.8.3",
		"handlebars-runtime": "~1.0.12",
		"express3-handlebars": "~0.5.0",
		"MD5": "~1.2.0"
	},
	"devDependencies": {//项目开发依赖包列表
		"bower": "~1.2.8",
		"grunt": "~0.4.1",
		"grunt-contrib-concat": "~0.3.0",
		"grunt-contrib-jshint": "~0.7.2",
		"grunt-contrib-uglify": "~0.2.7",
		"grunt-contrib-clean": "~0.5.0",
		"browserify": "2.36.1",
		"grunt-browserify": "~1.3.0",
	}
}
```
## 依赖包版本格式:
1. 指定：1.2.2
2. `~`+指定：~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2，小于1.3.x）
3. `^` + 指定版本：^1.2.2，表示安装1.x.x的最新版本，（不低于1.2.2，小于2.x.x）
4. latest：最新版本
`## package.json`生成方式：
1. 手写
2. 执行`npm init`生成
## 依赖包写入package.json
不在`package.json`的包要写入，使用--save 或者--save-dev
```
npm install express --save     # 将该模块写入dependencies属性
npm install express --save-dev   #将该模块写入devDependencies属性
```
# 下载安装、卸载、更新包
***
## 安装方式（全局安装与本地安装）
```
npm install express          # 本地安装
npm i express              #简写
npm install express -g   # 全局安装
```
* 全局安装：安装包放在 /usr/local 下或者你 node 的安装目录，可以在命令行直接使用
* 本地安装：安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），没有时会自动创建，通过 require() 来引入本地安装的包
## 引用下载的包
```
//var <Module Name>= require('<Module Name>');
var express = require('express');
```
## 卸载包
```
npm uninstall express   #删除node_modules目录下面的包
npm uninstall --save express      #删除node_modules的包和package.json中的运行时依赖
npm uninstall --save-dev express      #删除node_modules的包和package.json中的开发依赖
npm uninstall -g express #全局卸载
```
## 更新包
```
npm update express
npm update express -g # 更细全局包
```
# 查询包的信息
***

## 查看安装包信息
```
npm list    #查看本地安装包信息
npm list -g  #查看全局安装包信息
npm list express #查看某个安装包信息
npm ls    #npm list简写
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-77025bd3552afff9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 提升下载包的速度
由于npm的包大多是在国外数据库，下载速度会受到影响，我们想快一点，可以使用国内的淘宝镜像。
淘宝NPM镜像是一个完成的npmjs.org镜像，基本与官网服务一致。
## 镜像地址
淘宝镜像：
*   搜索地址：[http://npm.taobao.org/](http://npm.taobao.org/)
*   registry地址：[http://registry.npm.taobao.org/](http://registry.npm.taobao.org/)
官网镜像：
*  搜索地址：[https://www.npmjs.com/](https://www.npmjs.com/)
*   registry地址：[https://registry.npmjs.org/](https://registry.npmjs.org/)
## 使用淘宝镜像
### 临时使用淘宝镜像
```
#安装包时临时制定镜像地址
npm --registry https://registry.npm.taobao.org install express
```
### 持久使用淘宝镜像
先配置npm镜像，然后再安装包
配置镜像：
```
npm config set registry https://registry.npm.taobao.org  #设置成淘宝镜像
npm config set registry https://registry.npmjs.org/    #设置成官网的
```
监测是否设置成功
```
npm config get registry   #查询镜像地址
npm info express #查询镜像地址
```
### 使用cnpm来使用淘宝镜像
安装cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
使用cnpm安装包
```
cnpm install [name]
```
## 使用nrm来管理切换npm源
nrm专门用来管理和快速切换私人配置的registry。
### 安装
```
npm install -g nrm
```
### 列出可选源
```
nrm ls
```
### 切换源
```
nrm use taobao
```
### 增加源
```
nrm add <源名称> <源地址>   #比如企业或组织有自己的私有源（镜像）时
```
### 删除源
```
nrm del
```
### 测试源响应
```
nrm test   #测试所有源
nrm test npm  #测试npm官方源
```
# 如何创建和发布自己的包
***
## 创建模块
```
npm init
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-39b52d3d66dece35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时，生成了`package.json`配置文件
## 增加入口文件
默认的入口文件是根目录下的index.js，在根目录下创建index.js
```
//index.js
exports.printMsg = function() {
    console.log("This is a message from the demo package");
}
```
## 登录或注册npm账号
```
npm adduser  #注册npm账号
npm login #登录npm账号
```
## 发布包
发布包之前，我们需要做两步：
1. 修改CHANGE.MD，这里记录了我们包发布的版本变化情况，格式自定
2. 修改`package.json`中的`version`字段，表示这次发布的包的版本，如果不修改，发布会报错。
发布包：
```
npm publish
```
## 撤销发布
撤销发布自己发布过的某个版本代码
```
npm unpublish <package>@<version>
```
发布成功之后，这个包就可以通过`npm install`命令来进行安装了。
# 管理包的版本
当我们下载和发布我们的包时，都会关注到包的版本号，npm使用语义版本号来管理包。
语义版本号组成：X.Y.Z
* X代表主版本号，表示有大变动，向下不兼容
* Y代表次版本号，表示新增功能，向下兼容
* Z代表补丁版本号，表示修复BUG


参考资料：
官网：https://www.npmjs.com.cn/getting-started/installing-npm-packages-locally/
菜鸟教程：https://www.runoob.com/nodejs/nodejs-npm.html
npm脚本使用：http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html
package.json说明：http://javascript.ruanyifeng.com/nodejs/packagejson.html