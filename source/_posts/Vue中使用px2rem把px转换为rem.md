---
title: Vue中使用px2rem把px转换为rem
date: 2019-01-01 13:27:07
tags:
    -Vue
---

## Vue中使用px2rem把px转换为rem
使用Vue做移动端页面时，适配是必须的，rem不失为一种好的选择，可在不同屏幕上完美显示相同的布局
px2rem插件可以使`<style></style>`中的px单位根据设计稿转换为rem

### 1.安装
> npm install px2rem-loader -S

### 2.配置`px2rem-loader`
在`build/utils.js`的15行`export.assetsPath = function(_pth){...}`里面添加：

```JavaScript
    export.cssLoaders = funtion(options){
        option = option || {}
        const cssLoader = {
            loader:'css-loader',
            options:{
                sourceMap:options.sourceMap
            }
        }
        //下面为添加的代码
        const px2RemLoader = {
            loader:'px2rem-loader',
            options:{
                remUnit:75 //设计稿宽度的10%
            }
        }
        const postcssLoader = {
            loader:'postcss=loader',
            options:{
                sourceMap:options.sourceMap
            }
        }
    }
```
### 3.修改`function generateLoader(loader,loaderOptions){...}`    
将`const loader = ...`后面的三元运算符问好后面的数组每一个都加上`px2remLoader`
```JavaScript
    function generateLoaders(loader, loaderOptions){
        const loaders = option.usePostCss ? [cssLoader, postcssLoader, px2remLoader] : [cssLoader, px2remLoader]

        if(loader) {
            ....
        }
    }
```
### 4.使用
```html
    <style lang="" scoped>
        .wrap{
            width:750px; /** 上面的remUnit：75的时候为10rem **/
            height:1334px
        }
    </style>
```
### 5.注意事项
安装px2rem后，再使用px上有些不同，大家可以参考px2rem官方介绍，下面简单介绍一下。  
1，直接写px，编译后会直接转化成rem ---- 除开下面两种情况，其他长度用这个  

2，在px后面添加`/*no*/`，不会转化px，会原样输出。 --- 一般border需用这个    

3，在px后面添加`/*px*/`,会根据dpr的不同，生成三套代码。---- 一般字体需用这个

