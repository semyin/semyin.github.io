---
title: Vue在安卓低版本兼容问题
date: 2018-11-03 15:44:57
tags: 
    -Vue
---

## Vue在安卓低版本的兼容问题
在弄公司的项目的时候，打包完部署到测试环境上，发现有的低版本安卓有些样式不兼容，甚至有些数据请求不出来。

网上查了一些方法，现在列出来：
### 1.npm 安装 `bable-polyfill` `es6-promise`
>
    npm install babel-polyfill  
    npm install es6-promise

### 2.安装完在项目根目录会出现：

>
    "bable-polyfill":"^6.xxx",
    "es6-promise":"^4.xxx"

### 3.在项目入口文件`main.js`引入下面这段代码：

```Javascript
    "import 'bable-polyfill"
    "import 'es6-promise"
```

### 4.在`build`文件下的`webpack.base.conf.js`添加下面一段

```Javascript
    module.exports = {
        context: path.resolve(__dirname,'../'),
        entry:{
            // app:'./src/main.js'  //原来的注释掉 换成下面的
            app:["babel-polyfill","./src/main.js"]
        }
    }
```
### 5.重新打包部署，就可以支持低版本的安卓，我测试最低的支持5.0