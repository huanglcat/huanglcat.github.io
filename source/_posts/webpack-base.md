---
title: WebPack基础配置详解
date: 2019-05-21 21:58:25
categories: "WebPack" #文章分類目錄 可以省略
tags: [WebPack]
---
# WebPack是什么
定义：WebPack是__模块打包工具__。
原理：分析项目结构，找到JavaScript模块以及其他浏览器不能直接运行的模块（Scss，TypeScript等），转换并打包为浏览器可以识别并运行的格式，让浏览器使用。

![image.png](https://upload-images.jianshu.io/upload_images/15009210-a23211446cbd6a36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<!--more-->

工作流程：
1、通过配置找到给定的入口文件（如index.js）
2、从入口文件开始分析并处理项目所有的依赖模块，并递归地构建一个依赖关系图（dependency graph）。webpack把所有的文件都当做模块。
      * JavaScript模块：webpack自己本身就可以识别并处理
      * 其他模块：通过使用loaders来分析和转译浏览器不认识的模块为浏览器认识的格式
3、把所有的模块打包为一个或多个浏览器可识别的JavaScript文件，默认叫做bundle.js（也可以自己改名），根据给定的输出地址，输出到指定目录，一般叫做dist。
![image.png](https://upload-images.jianshu.io/upload_images/15009210-5ed62bb5fe2f977e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
webpack是基于node的，所以把静态文件都看成模块，通过模块化语言（esmodule,commonjs,require语法）识别模块的引入，当遇到模块引入时，webpack就知道这是一个依赖模块了。

## 官网链接：
英：[https://webpack.js.org/](https://webpack.js.org/)  【较新，webpack相关更新都会比较及时】
中：[https://www.webpackjs.com/](https://www.webpackjs.com/)

# WebPack使用
下面通过一系列详细实例来解析webpack配置。
## 安装
新建目录，在目录下执行命令：
```
npm init -y  #初始化一个package.json
npm install webpack webpack-cli -D    #本地安装最新版`webpack`,`webpack-cli`
npm info webpack   #查看webpack历史版本信息（当要安装特定版本时）
```
## webpack打包Js模块
由于最新版的webpack是支持零配置，默认的入口文件是src/index.js，因此我们新建一个src文件夹，新建两个js文件，一个默认的入口文件`index.js`，一个`a.js`：
```
//index.js
import a from "./a"
a()
```
```
//a.js
function a(){
    console.log("hello,我是a");
}
 export default a;
```
新建一个`index.html`:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <script src="./src/index.js"></script>
</body>
</html>
```
目录现在如下：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-28e59c6647adfe0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这时候打开在浏览器打开html，浏览器是不识别import的，会报错，因此我们使用webpack来解析并打包js模块，运行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 4.878 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: b14a40261c3e17ea5995
Version: webpack 4.31.0
Time: 355ms
Built at: 2019-05-19 15:42:16
  Asset       Size  Chunks             Chunk Names
main.js  976 bytes       0  [emitted]  main
Entrypoint main = main.js
[0] ./src/index.js + 1 modules 97 bytes {0} [built]
    | ./src/index.js 24 bytes [built]
    | ./src/a.js 73 bytes [built]

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set
'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
```
>webpack命令行打包的命令：
>```
>webpack            #打包，默认入口文件为src/index.js
>webpack index.js           #打包，指定入口文件路径
>```
可以看到webpack把js模块打包了并输出了一个`main.js`，现在目录下多了一个dist文件夹：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-3180ed0f1656b1e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
把index.html里引入的js路径修改为dist/main.js，在浏览器打开，就可以看到js正确执行结果：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-a3760e8e93ef3f9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在运行npx webpack时，我们可以看到有一个警告：
```
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set
'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
```
意思是说我们没有设置mode选项，这是webpack的打包模式，默认是“production”就是生产打包选项，production模式会自自动压缩打包后的文件，我们可以把mode设置为其他模式，设置为开发模式（development）：
```
const path = require('path');
module.exports = {
    mode:"development"
}
```
再次运行时，可以看到输出的文件不再是压缩后的文件。

## webpack配置文件
零配置比较局限，要有更多的功能需要手动配置，根目录下新增一个`webpack.config.js`：
```
//webpack.config.js
//是一个对象
module.exports = {
}
```
为什么要是webpack.config.js呢，这是webpack定义的一个默认的配置文件名，webpack运行时会检查是否有这个配置文件，有这个配置文件时，会使用其中的配置来覆盖默认配置。
## entry & output
入口和输出文件目录是可以自己设定的，输入以下内容：
```
//webpack.config.js
const path = require('path');
module.exports = {
    //entry:"./src/index.js",   #入口文件路径配置
    entry:{   #入口文件配置可以使用字符串模式也可以使用对象模式
        main:'./src/index.js'
    },
    output:{     #输出文件路径配置
        filename:'bundle.js',   #输出文件名
        //output的path需要是绝对路径
        path:path.resolve(__dirname,'dist')          #输出文件路径，绝对路径，使用node.js的path模块来解析为绝对路径，这里设置为根目录下的dist目录
    },
}
```
这里修改了出口文件的名字，删除dist文件夹后，再次运行`npx webpack`，可以看到dist里的文件已经变成了bundle.js
![image.png](https://upload-images.jianshu.io/upload_images/15009210-d9fc7a34e8908873.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 什么是loader
上面已经说到，webpack默认只能识别JS模块，其他模块是不识别的。
而loader就是帮助webpack来识别并解析除了JS的其他模块的。
针对各种各样的模块，有各种的loader。
loader是一个声明式函数。
当webpack中有不是JS的模块时，需要配置对应的loader，监测对应的模块格式，使用对应的loader处理。loader的配置主要在module.rules中进行，这是一个数组，里面是各种loader的规则。
每一个loader的主要工作机制：
1. 识别文件类型，确定具体处理该模块的loader（rule.test）
2. 使用对应的loader，对文件进行相关操作转换（rule.use）
loader的在配置文件中的设置：
```
//webpack.config.js
const path = require('path');
module.exports = {
    entry:{
        main:'./src/index.js'
    },
    output:{
        filename:'bundle.js',
        path:path.resolve(__dirname,'dist')
    },
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            {
                test:/\.xxx$/,    //表示匹配规则，是一个正则表达式
                use:{     //表示针对匹配文件将使用处理的loader
                    loader:"xxx-loader",
                    options:{   //loader的可配置项
                    }
                }
            }
        ]
    },
}
```
常用的loader列举：
* 处理静态文件：file-loader、url-loader、raw-loader
* 处理样式模块：style-loader、css-loader、sass-loader、less-loader、postcss-loader
* 处理数据文件：csv-loader、xml-loader
* 处理模块语言：html-loader、markdown-loader
* 处理测试模块：mocha-loader、eslint-loader
## 静态资源模块loader：file-loader
静态资源，比如图片、文件，使用file-loader来处理
>file-loader的工作机制：
>1. 把打包入口中识别出来的静态模块直接复制到输出目录（dist）下；
>2. 返回一个复制的文件的地址名称（文件名）
>
>使用场景：只需要把文件移动到输出目录下，不需要处理，比如图片、excel、word、svg等等。
在`index.js`中引入一个图片，图片是静态文件，webpack是不认识的，需要file-loader处理
```
import a from "./a"
a()
var pic = require('./pic.jpg');
console.log(pic);
```
先安装：
```
npm i file-loader -D
```
配置：
```
const path = require('path');
module.exports = {
    entry:"./src/index.js",
    // entry:{
    //     main:'./index.js'
    // },
    output:{
        filename:'bundle.js',
        //绝对路径
        path:path.resolve(__dirname,'dist')
    },
    mode:"development",
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            {
                test:/\.(jpe?g|png|gif)$/,     
                use:{
                    loader:"file-loader"
                }
            }
        ]
    },
}
```
执行`npx webpack`可以看到文件正常打包，dist里面多了一个图片：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 4.322 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: 2ef4598423ef35d9401b
Version: webpack 4.31.0
Time: 409ms
Built at: 2019-05-19 21:43:02
                               Asset      Size  Chunks             Chunk Names
91d98190304e9e52a3b55d37b5f60397.jpg  8.89 KiB          [emitted]
                           bundle.js  4.86 KiB    main  [emitted]  main
Entrypoint main = bundle.js
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 59 bytes {main} [built]
[./src/pic.jpg] 82 bytes {main} [built]
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-dd072f5314375baf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

复制的文件名是可以改的，输出位置也可以通过options选项可以配置：
```
module:{
        rules:[
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"file-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符（placeholder），name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",    #配置输出位置
                    }
                }
            }
        ]
    },
```
运行命令打包如下：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 14.264 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: faabe6751318c1104e28
Version: webpack 4.31.0
Time: 294ms
Built at: 2019-05-19 21:57:04
                                          Asset      Size  Chunks             Chunk Names
                                      bundle.js  4.89 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg  8.89 KiB          [emitted]
Entrypoint main = bundle.js
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 77 bytes {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-20db8b03d590c17c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>很多loader都有一个叫做placeholder（占位符）的概念，可以有不同的占位符，比如名称、后缀、hash等。
## 静态模块：url-loader
>url-loader可以做file-loader可以做的所有事情，多出来的功能是：默认把静态资源转成base64格式并打包到bundle.js(最终的打包文件)，可以使用limit选项来设置是否转译范围。大于limit范围的不转译。
安装：
```
npm i url-loader -D
```
配置：
```
module:{
        rules:[
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"url-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",
                        limit:2048,//当url-loader处理jpg模块，会判断体积是否在限制范围之内，在limit之内的，就转成base64格式并打包到bundle.js，否则就不转直接输出到dist
                    }
                }
            }
        ]
    },
```
运行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 5.906 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: 74ba975fbbf2f7d18da1
Version: webpack 4.31.0
Time: 328ms
Built at: 2019-05-19 22:02:46
    Asset      Size  Chunks             Chunk Names
bundle.js  16.7 KiB    main  [emitted]  main
Entrypoint main = bundle.js
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 77 bytes {main} [built]
[./src/pic.jpg] 11.9 KiB {main} [built]
```
这时候我们的dist目录下没有了图片了，在bundle.js中却多出来一堆base64的字符串，url-loader默认把图片转成了base64字符串。
当图片小的时候，用url-loader可以减少请求数，但是图片大的时候，这样会增大打包文件的体积，所以需要有个度，可以使用options中的limit字段来设置文件是否转base64的限制，在限制范围内，就转译base64并打包到最终打包文件bundle.js，否则，就直接执行file-loader的功能，直接移动到输出目录。
## 样式文件模块：
解释style-loader、css-loader、sass-loader、postcss-loader
### 处理css模块
处理css模块需要两个loader：style-loader和css-loader
>css-loader工作机制：处理css模块，识别合并css模块

>style-loader工作机制：把合并的css模块放到html中head的style标签中。

安装
```
npm i style-loader css-loader -D
```
配置：
```
    module:{
        rules:[
            {
                test:/\.css$/,
                use: [   #执行顺序：从下到上，从右到左
                    'style-loader',   #把合并的css放到style标签
                    'css-loader',    #先识别css并合并为一个css
                  ],
            },
        ]
    },
```
在src目录中增加`index.css`
```
body{
  background:blue;
}
```
在`index.js`中引入`index.css`
```
import './index.css';
```
运行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 5.363 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: 01f1fa305f332e6c34fc
Version: webpack 4.31.0
Time: 962ms
Built at: 2019-05-19 22:23:28
                                          Asset      Size  Chunks             Chunk Names
                                      bundle.js  24.6 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg  8.89 KiB          [emitted]
Entrypoint main = bundle.js
[./node_modules/css-loader/dist/cjs.js!./src/index.css] 175 bytes {main} [built]
[./src/a.js] 73 bytes {main} [built]
[./src/index.css] 1.06 KiB {main} [built]
[./src/index.js] 102 bytes {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
    + 3 hidden modules
```
可以看到成功打包了index.css，在浏览器打开index.html，可以看到css生效了：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-6b9d6711ad08bc74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 处理scss模块
样式文件有很多种写法，这里以sass为例，sass的文件后缀是scss，处理sass文件需要style-loader、css-loader、sass-loader，
安装：
```
#sass-loader把sass语法转换成css，依赖node-sass模块
npm i scss-loader node-sass -D   
```
配置：
```
    module:{
        rules:[
            {
                test:/\.scss$/,
                use:[
                    'style-loader',//放在正确的打包位置
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                ]
            }
        ]
    },
```
在src下新增`index.scss`:
```
html{
    body{
        background:blue;
    }
}
```
修改`index.js`，引入`index.scss`
```
import a from "./a"
a()

var pic = require('./pic.jpg');
console.log(pic)

// import './index.css';
import './index.scss';
```
删除dist目录，执行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 15.772 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: b704540cce4d7e089aea
Version: webpack 4.31.0
Time: 3343ms
Built at: 2019-05-20 06:29:40
                                          Asset      Size  Chunks             Chunk Names
                                      bundle.js  24.9 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg  8.89 KiB          [emitted]
Entrypoint main = bundle.js
[./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./src/index.scss] 176 bytes {main} [built]
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 129 bytes {main} [built]
[./src/index.scss] 1.18 KiB {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
    + 3 hidden modules
```
可以看到成功打包了scss模块，在浏览器打开`index.html`，样式成功渲染：

### 自动给样式增加前缀：postcss-loader
一般来说开发之中还会对样式模块的loader增加一个处理：自动给样式增加前缀（-webkit-，-o-等浏览器兼容前缀），这样就不用自己一个个的写了。
postcss-loader可以自定识别需要增加前缀的样式，自动给他们增加前缀代码。
安装：
```
#autoprefixer是增加前缀的依赖包
npm i postcss-loader autoprefixer -D
```
配置：
```
    module:{
        rules:[
            {
                test:/\.scss$/,
                use:[
                    'style-loader',//放在正确的打包位置
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                    'postcss-loader'//自定增加前缀
                ]
            }
        ]
    },
```
增加postcss-loader配置文件，在根目录下新建`postcss.config.js`
```
module.exports = {
    plugins:[
        require('autoprefixer')
    ]
}
```
在`index.html`中修改：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div class="demo">
    </div>
    <script src="./dist/bundle.js"></script>
</body>
</html>
```
在`index.scss`增加css3代码：
```
html{
    body{
        background: green;
        .demo{
            width:200px;
            height:200px;
            background: red;
            transform:translate(100px,100px)
        }
    }
}
```
运行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 4.451 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: 6d8a1bc0a093edc5c610
Version: webpack 4.31.0
Time: 1342ms
Built at: 2019-05-20 06:44:47
                                          Asset      Size  Chunks             Chunk Names
                                      bundle.js  25.3 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg  8.89 KiB          [emitted]
Entrypoint main = bundle.js
[./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src/index.js!./src/index.scss] ./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src!./src/index.scss 350 bytes {main} [built]
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 129 bytes {main} [built]
[./src/index.scss] 1.31 KiB {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
    + 3 hidden modules
```
可以看到成功打包了，浏览器刷新：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-81f11f4ccfe0951a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到css中自动增加了浏览器兼容前缀代码。
## 什么是Plugins
Plugins是开始打包时，在某个时刻，帮助我们处理一些什么事情的机制，它是事件驱动的，本身是一个类。
一种插件就是一种函数，通过传入不同的参数，可以实现不同的功能。
### 自动生成html
>HtmlWebpackPlugin是在打包后的时刻，自动帮你生成一个引入了打包后的JS的html，并放到输出目录下的一个插件。

安装
```
npm i html-webpack-plugin -D
```
配置：
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry:"./src/index.js",
    // entry:{
    //     main:'./index.js'
    // },
    output:{
        filename:'bundle.js',
        //绝对路径
        path:path.resolve(__dirname,'dist')
    },
    mode:"development",
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            // {
            //     test:/\.(jpe?g|png|gif)$/,
            //     use:{
            //         loader:"file-loader",
            //         options:{
            //             name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
            //             outputPath:"images/",
            //         }
            //     }
            // },
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"url-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",
                        limit:2048,//当url-loader处理jpg模块，会判断体积是否在限制范围之内，在limit之内的，就转成base64格式并打包到bundle.js，否则就不转直接输出到dist
                    }
                }
            },
            {
                test:/\.scss$/,
                use:[
                    'style-loader',//放在正确的打包位置
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                    'postcss-loader'
                ]
            }

        ]
    },
    //配置插件，是个数组，里面的项是插件的实例
    plugins:[
        //自动生成html，并移到输出目录
        new HtmlWebpackPlugin({
            title:'html模板',  
            filename:'index.html',
            template:"./index.html"     #生成html的模板路径
        }),
    ],

}
```
删除dist目录，删除html模板（此时根目录下的index.html）中自己加上去的script引入，运行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 12.055 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: 72bd3988b3ab5a7aeb46
Version: webpack 4.31.0
Time: 4965ms
Built at: 2019-05-20 21:13:57
                                          Asset       Size  Chunks             Chunk Names
                                      bundle.js   25.3 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg   8.89 KiB          [emitted]
                                     index.html  406 bytes          [emitted]
Entrypoint main = bundle.js
[./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src/index.js!./src/index.scss] ./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src!./src/index.scss 350 bytes {main} [built]
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 129 bytes {main} [built]
[./src/index.scss] 1.31 KiB {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
    + 3 hidden modules
Child html-webpack-plugin for "index.html":
     1 asset
    Entrypoint undefined = index.html
    [./node_modules/html-webpack-plugin/lib/loader.js!./index.html] 569 bytes {0} [built]
    [./node_modules/webpack/buildin/global.js] (webpack)/buildin/global.js 472 bytes {0} [built]
    [./node_modules/webpack/buildin/module.js] (webpack)/buildin/module.js 497 bytes {0} [built]
        + 1 hidden module
```
成功打包，输出的dist目录下多了一个index.html，里面自动加上了打包后的js
![image.png](https://upload-images.jianshu.io/upload_images/15009210-69d460106ad91d2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 自动删除dist目录
我们每次在打包时，文件都是已覆盖已有的，不删除多余的。所以dist目录如果不手动删除，就会有很多多余文件，因此我们可以使用一个插件帮助我们执行删除dist目录的操作。
>CleanWebpackPlugin插件是在打包之前，自动帮我们删除dist目录，以免污染打包环境。

安装：
```
npm i clean-webpack-plugin -D
```
配置：
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
module.exports = {
    entry:"./src/index.js",
    // entry:{
    //     main:'./index.js'
    // },
    output:{
        filename:'bundle.js',
        //绝对路径
        path:path.resolve(__dirname,'dist')
    },
    mode:"development",
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            // {
            //     test:/\.(jpe?g|png|gif)$/,
            //     use:{
            //         loader:"file-loader",
            //         options:{
            //             name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
            //             outputPath:"images/",
            //         }
            //     }
            // },
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"url-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",
                        limit:2048,//当url-loader处理jpg模块，会判断体积是否在限制范围之内，在limit之内的，就转成base64格式并打包到bundle.js，否则就不转直接输出到dist
                    }
                }
            },
            {
                test:/\.scss$/,
                use:[
                    'style-loader',//放在正确的打包位置
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                    'postcss-loader'
                ]
            }

        ]
    },
    plugins:[   #执行顺序从上到下
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title:'html模板',
            filename:'index.html',
            template:"./index.html"
        }),
    ],
}
```
### 自动导出css
如果不想让css放在style标签中，我们可以使用插件来帮我们抽离css并打包输出到dist目录。
>MiniCssExtractPlugin可以帮我们代替style-loader，抽离css，并输出到dist目录

安装：
```
npm i mini-css-extract-plugin -D
```
配置：
```
 const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
    entry:"./src/index.js",
    // entry:{
    //     main:'./index.js'
    // },
    output:{
        filename:'bundle.js',
        //绝对路径
        path:path.resolve(__dirname,'dist')
    },
    mode:"development",
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            // {
            //     test:/\.(jpe?g|png|gif)$/,
            //     use:{
            //         loader:"file-loader",
            //         options:{
            //             name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
            //             outputPath:"images/",
            //         }
            //     }
            // },
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"url-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",
                        limit:2048,//当url-loader处理jpg模块，会判断体积是否在限制范围之内，在limit之内的，就转成base64格式并打包到bundle.js，否则就不转直接输出到dist
                    }
                }
            },
            {
                test:/\.scss$/,
                use:[
                    {
                      loader: MiniCssExtractPlugin.loader,
                      options: {
                        publicPath: '../',
                        hmr: process.env.NODE_ENV === 'development',
                      },
                    },
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                    'postcss-loader'
                ]
            }

        ]
    },
    plugins:[
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title:'html模板',
            filename:'index.html',
            template:"./index.html"
        }),
        new MiniCssExtractPlugin({
            filename: '[name].css',        #输出文件名 
            chunkFilename: '[id].css',            #模块名
        }),
    ],

}
```
执行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 3.619 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: dd2b22f9beb8876949e7
Version: webpack 4.31.0
Time: 2052ms
Built at: 2019-05-20 21:45:17
                                          Asset       Size  Chunks             Chunk Names
                                      bundle.js    5.5 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg   8.89 KiB          [emitted]
                                     index.html  399 bytes          [emitted]
                                       main.css  204 bytes    main  [emitted]  main
Entrypoint main = main.css bundle.js
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 127 bytes {main} [built]
[./src/index.scss] 39 bytes {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
    + 1 hidden module
Child html-webpack-plugin for "index.html":
     1 asset
    Entrypoint undefined = index.html
    [./node_modules/html-webpack-plugin/lib/loader.js!./index.html] 521 bytes {0} [built]
    [./node_modules/webpack/buildin/global.js] (webpack)/buildin/global.js 472 bytes {0} [built]
    [./node_modules/webpack/buildin/module.js] (webpack)/buildin/module.js 497 bytes {0} [built]
        + 1 hidden module
Child mini-css-extract-plugin node_modules/css-loader/dist/cjs.js!node_modules/sass-loader/lib/loader.js!node_modules/postcss-loader/src/index.js!src/index.scss:
    Entrypoint mini-css-extract-plugin = *
    [./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src/index.js!./src/index.scss] ./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src!./src/index.scss 350 bytes {mini-css-extract-plugin} [built]
        + 1 hidden module
```
输出日志中可以看到mini-css-extract-plugin的工作流程，dist目录下多了一个main.css文件，输出的index.html中自动增加了main.css。
![image.png](https://upload-images.jianshu.io/upload_images/15009210-b6e6dd672ec5d3fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 开发模式排查问题：SourceMap
默认情况下，如果代码有了错误，浏览器会直接定位到bundle.js，很难找到是哪里出了错，但是我们可以通过开启SourceMap来帮助我们定位错误。
mode为development时，是默认开启SourceMap的，当关闭开发模式时，我们可以使用devtool来设置SourceMap
>devtool是设置打包时的文件映射，文件映射越完整，打包速度越慢，devtool可以设置各种速度和程度的打包及文件映射。

设置：
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
    entry:"./src/index.js",
    // entry:{
    //     main:'./index.js'
    // },
    output:{
        filename:'bundle.js',
        //绝对路径
        path:path.resolve(__dirname,'dist')
    },
    mode:"development",
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            // {
            //     test:/\.(jpe?g|png|gif)$/,
            //     use:{
            //         loader:"file-loader",
            //         options:{
            //             name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
            //             outputPath:"images/",
            //         }
            //     }
            // },
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"url-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",
                        limit:2048,//当url-loader处理jpg模块，会判断体积是否在限制范围之内，在limit之内的，就转成base64格式并打包到bundle.js，否则就不转直接输出到dist
                    }
                }
            },
            {
                test:/\.scss$/,
                use:[
                    {
                      loader: MiniCssExtractPlugin.loader,
                      options: {
                        publicPath: '../',
                        hmr: process.env.NODE_ENV === 'development',
                      },
                    },
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                    'postcss-loader'
                ]
            }

        ]
    },
    plugins:[
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title:'html模板',
            filename:'index.html',
            template:"./index.html"
        }),
        new MiniCssExtractPlugin({
            filename: '[name].css',      //输出文件名
            chunkFilename: '[id].css',          //模块名
        }),
    ],
    devtool:'source-map'     //设置文件映射
}
```
执行命令：
```
F:\front-end\webpack\webpack-demos>npx webpack
npx: 1 安装成功，用时 4.319 秒
'prefix' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
Hash: d599b2c86a9ac5ae3802
Version: webpack 4.31.0
Time: 1930ms
Built at: 2019-05-20 21:52:50
                                          Asset       Size  Chunks             Chunk Names
                                      bundle.js   5.29 KiB    main  [emitted]  main
                                  bundle.js.map   4.19 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg   8.89 KiB          [emitted]
                                     index.html  399 bytes          [emitted]
                                       main.css  240 bytes    main  [emitted]  main
                                   main.css.map  401 bytes    main  [emitted]  main
Entrypoint main = main.css bundle.js main.css.map bundle.js.map
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 127 bytes {main} [built]
[./src/index.scss] 39 bytes {main} [built]
[./src/pic.jpg] 93 bytes {main} [built]
    + 1 hidden module
Child html-webpack-plugin for "index.html":
     1 asset
    Entrypoint undefined = index.html
    [./node_modules/html-webpack-plugin/lib/loader.js!./index.html] 521 bytes {0} [built]
    [./node_modules/webpack/buildin/global.js] (webpack)/buildin/global.js 472 bytes {0} [built]
    [./node_modules/webpack/buildin/module.js] (webpack)/buildin/module.js 497 bytes {0} [built]
        + 1 hidden module
Child mini-css-extract-plugin node_modules/css-loader/dist/cjs.js!node_modules/sass-loader/lib/loader.js!node_modules/postcss-loader/src/index.js!src/index.scss:
    Entrypoint mini-css-extract-plugin = *
    [./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src/index.js!./src/index.scss] ./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src!./src/index.scss 350 bytes {mini-css-extract-plugin} [built]
        + 1 hidden module
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-a4074adf0014ee2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到dist目录里多了后缀为map的文件，这些文件就是打包文件与源码的映射文件，有助于我们定位错误，调试代码。
开发模式下我们可以使用模式“cheap-module-eval-source-map”，这个模式定位错误到行，并且可以定位第三方模块的错误并输出。
## 本地服务器：devServer
前端开发离不开这个利器：本地服务器，可以配置devServer并配合插件实现修改代码浏览器同步刷新，实时查看修改效果。
>WebpackDevServer

安装：
```
npm i webpack-dev-server -D
```
修改package.json
```
  "scripts": {
    "server": "webpack-dev-server",
  },
```
配置devServer：
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    // entry:"./index.js",
    entry:{
        main:'./index.js'
    },
    output:{
        filename:'bundle.js',
        //绝对路径
        path:path.resolve(__dirname,'dist')
    },
    devServer:{
        contentBase:'./dist',    //服务器启动的目录
        open:true,   //自动打开浏览器
        proxy:{    //设置代理，可用于本地mock数据，本地自己启动另外一个服务
            "/api":{
                target:"http://localhost:9092"
            }
        },
        port:8083, //指定端口号
        hot:true,   //开启HMR(Hot Module Replacement)热模块替换,由于是webpack自带的，所以要引入webpack ，监控并更新js模块的工作vue等框架自己做了，否则需要自己手动监控 
        hotOnly:true
    },
    //不认识的模块（不是js)的配置
    module:{
        rules:[
            {
                test:/\.js$/,exclude:/node_modules/,
                loader:"babel-loader",
                options:{
                    // "presets":[["@babel/preset-env",{
                    //     useBuiltIns:"usage"
                    // }]]
                    // plugins:[["@babel-plugin-transform-runtime",{
                    //     "absolute"
                    // }]
                }
            },
            {
                test:/\.(jpe?g|png|gif)$/,
                use:{
                    loader:"url-loader",
                    options:{
                        name:"[name]-[hash].[ext]",//[]表示占位符，name表示源文件的名字，ext是源文件的后缀，还可以连接hash：[name]-[hash].[ext]
                        outputPath:"images/",
                        limit:2048,//当webpack处理jpg模块，会判断体积是否在限制范围之内，y限制在体积小于2048的
                    }
                }
            },
            {
                test:/\.css$/,
                use: [
                    // {
                    //   loader: MiniCssExtractPlugin.loader,
                    //   options: {
                    //     // you can specify a publicPath here
                    //     // by default it uses publicPath in webpackOptions.output
                    //     publicPath: '../',
                    //     hmr: process.env.NODE_ENV === 'development',
                    //   },
                    // },
                    'style-loader',
                    'css-loader',
                    'postcss-loader'
                  ],
                // [
                //     'style-loader',
                //     'css-loader',
                //     'postcss-loader'
                // ]
            },
            {
                test:/\.scss$/,
                use:[
                    'style-loader',//放在正确的打包位置
                    'css-loader',//打包合并css
                    'sass-loader',//处理sass，转成css
                    'postcss-loader'
                ]
            }
        ]
    },
    plugins:[
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title:'html模板',
            template:"./index.html"
        }),
        // new MiniCssExtractPlugin({
        //     // Options similar to the same options in webpackOptions.output
        //     // both options are optional
        //     filename: '[name].css',
        //     chunkFilename: '[id].css',
        // }),
        new webpack.HotModuleReplacementPlugin()
    ],
    devtool:'inline-source-map',//打包后的文件映射
    mode:"development"//去掉警告，开发模式
}
```
执行命令：
```
F:\front-end\webpack\webpack-demos>npm run server

> demo@1.0.0 server F:\front-end\webpack\webpack-demos
> webpack-dev-server

i ｢wds｣: Project is running at http://localhost:8083/
i ｢wds｣: webpack output is served from /
i ｢wds｣: Content not from webpack is served from ./dist
i ｢wdm｣: Hash: 06f7a27fe38a8a9b134a
Version: webpack 4.31.0
Time: 4661ms
Built at: 2019-05-20 22:24:21
                                          Asset       Size  Chunks             Chunk Names
                                      bundle.js    367 KiB    main  [emitted]  main
                                  bundle.js.map    419 KiB    main  [emitted]  main
images/pic-91d98190304e9e52a3b55d37b5f60397.jpg   8.89 KiB          [emitted]
                                     index.html  399 bytes          [emitted]
                                       main.css  240 bytes    main  [emitted]  main
                                   main.css.map  401 bytes    main  [emitted]  main
Entrypoint main = main.css bundle.js main.css.map bundle.js.map
[0] multi (webpack)-dev-server/client?http://localhost:8083 (webpack)/hot/only-dev-server.js ./src/index.js 52 bytes {main} [built]
[./node_modules/loglevel/lib/loglevel.js] 7.68 KiB {main} [built]
[./node_modules/querystring-es3/index.js] 127 bytes {main} [built]
[./node_modules/url/url.js] 22.8 KiB {main} [built]
[./node_modules/webpack-dev-server/client/index.js?http://localhost:8083] (webpack)-dev-server/client?http://localhost:8083 9.26 KiB {main} [built]
[./node_modules/webpack-dev-server/client/overlay.js] (webpack)-dev-server/client/overlay.js 3.59 KiB {main} [built]
[./node_modules/webpack-dev-server/client/socket.js] (webpack)-dev-server/client/socket.js 1.05 KiB {main} [built]
[./node_modules/webpack-dev-server/node_modules/strip-ansi/index.js] (webpack)-dev-server/node_modules/strip-ansi/index.js 161 bytes {main} [built]
[./node_modules/webpack/hot sync ^\.\/log$] (webpack)/hot sync nonrecursive ^\.\/log$ 170 bytes {main} [built]
[./node_modules/webpack/hot/emitter.js] (webpack)/hot/emitter.js 75 bytes {main} [built]
[./node_modules/webpack/hot/log-apply-result.js] (webpack)/hot/log-apply-result.js 1.27 KiB {main} [built]
[./node_modules/webpack/hot/log.js] (webpack)/hot/log.js 1.11 KiB {main} [built]
[./node_modules/webpack/hot/only-dev-server.js] (webpack)/hot/only-dev-server.js 2.55 KiB {main} [built]
[./src/a.js] 73 bytes {main} [built]
[./src/index.js] 127 bytes {main} [built]
    + 17 hidden modules
Child html-webpack-plugin for "index.html":
     1 asset
    Entrypoint undefined = index.html
    [./node_modules/html-webpack-plugin/lib/loader.js!./index.html] 521 bytes {0} [built]
    [./node_modules/lodash/lodash.js] 527 KiB {0} [built]
    [./node_modules/webpack/buildin/global.js] (webpack)/buildin/global.js 472 bytes {0} [built]
    [./node_modules/webpack/buildin/module.js] (webpack)/buildin/module.js 497 bytes {0} [built]
Child mini-css-extract-plugin node_modules/css-loader/dist/cjs.js!node_modules/sass-loader/lib/loader.js!node_modules/postcss-loader/src/index.js!src/index.scss:
    Entrypoint mini-css-extract-plugin = *
    [./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src/index.js!./src/index.scss] ./node_modules/css-loader/dist/cjs.js!./node_modules/sass-loader/lib/loader.js!./node_modules/postcss-loader/src!./src/index.scss 350 bytes {mini-css-extract-plugin} [built]
    [./node_modules/css-loader/dist/runtime/api.js] 2.35 KiB {mini-css-extract-plugin} [built]
i ｢wdm｣: Compiled successfully.
```
自动打开浏览器：
![image.png](https://upload-images.jianshu.io/upload_images/15009210-8ca6eab27b5a43b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
