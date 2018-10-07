---
title: vscode下.vue文件初始化
date: 2018-10-06 16:38:53
tags:
    -Vue
    -vscode
categories:
    -Vue相关
---

## vscode下.vue文件初始化
当我们在使用vscode编写vue文件的时候，每次都需要输入`<template></template>`,`<script></script>`,`<style></style>`这些标签  
如何像我们之前一样写html使用emmet插件一样使用 `!`自动出来html的格式呢
<!-- more -->
### 1 安装Vetur扩展让VScode支持.vue文件名
### 2 然后打开 菜单栏=>Code=>首选项=>用户代码片段=>选择vue
### 3 打开后发现是一个json文件，里面都是注释，不用管它，我们再后面添加
```JavaScript
"Vue Init":{
        "prefix": "vue",
        "description": "初始化Vue单文件组件模板",
        "body": [
            "<template>",
            "$1",
            "</template>",
            "<script>",
            "export default {",
            "   name:'$2',",
            "}",
            "</script>",
            "<style scoped>",
            "$3",
            "</style>",
            ""
        ]
    }
```
### 4 新建任一一个vue文件，第一行输入`vue`，会弹出来以下提示，按enter就ok了
![](/images/blog-img/18-10-7/1.png'描述')
<img src="/images/blog-img/18-10-7/1.png">
