---
title: 'Vue-cli创建项目的时候卡在下载谷歌驱动问题'
date: 2018-11-03 16:13:15
tags:
    -Vue
---

## npm 安装chromedriver失败解决办法

在使用Vue-cli新建一个Vue项目的时候，在做完一系列项目init的操作完之后  
`npm install`有时会出现:
```
Downloading https://chromedriver.storage.googleapis.com/2.27/chromedriver_mac64.zip
Saving to  /var/folders/7l/mhhqzhps0y59by7pf04nyx5r0000gn/T/chromedriver/chromedriver_mac64.zip
```
出现这个的时候，可能会一直卡在这里，什么也做不了，实际上项目的包已经安装好了  
经分析发现，某些版本下，chromedriver 的 zip 文件 url 的响应是 302 跳转，而在 install.js 里使用的是 Node.js 内置的 http 对象的 get 方法无法处理 302 跳转的情况；而在另外一些情况下，则是因为 googleapis.com 被墙了，此时即使采用科学上网的方法也仍然无法获取文件。

无论是上述哪种情况，可以使用下面的命令安装：

>npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver

或者用cnpm安装包依赖

文章来自:[segmentfault](https://segmentfault.com/a/1190000008310875)