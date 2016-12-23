---
title: webpack常用插件
date: 2016-11-30
tags:
categories: webpack
---

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务
。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

Webpack有很多内置插件，同时也有很多第三方插件，可以让我们完成更加丰富的功能。

<!-- more -->
### 1. 自动补全css3前缀 autoprefixer

官方是这样说的：Parse CSS and add vendor prefixes to CSS rules using values from the Can I Use website，也就是说它是一个自动检测兼容性给各个浏览器加个内核前缀的插件。

#### 使用方法:

```bash
npm install --save-dev autoprefixer postcss-loader

var autoprefixer = require('autoprefixer');
module.exports={
  //其他配置这里就不写了

  module:{
    loaders:[
    {
      test:/\.css$/,
      //在原有基础上加上一个postcss的loader就可以了
      loaders:['style-loader','css-loader','postcss-loader']
      }
      ]
  },
  postcss:[autoprefixer({browsers:['last 2 versions']})]
}
```
> autoprefixer默认的是没有兼容IE/opera的-ms/-o


```bash
例子：为浏览最新版本添加前缀，市场份额大于%，美国份额>5%，ie8和ie7
"browsers": ["last 1 version", "> 10%", "> 5% in US", "ie 8", "ie 7"]
```
参考写法：

* last 2 versions	每一个主要浏览器的最后2个版本
* last 2 Chrome versions	谷歌浏览器的最后两个版本
* > 5%	市场占有量大于5%
* > 5% in US	美国市场占有量大于5%
* ie 6-8	ie浏览器6-8
* Firefox > 20	火狐版本>20
* Firefox >= 20	火狐版本>=20
* Firefox < 20	火狐<20
* Firefox <= 20	火狐<=20
* iOS 7	指定IOS 7浏览器

更多请参考：https://github.com/ai/browserslist#queries

### 2.提取样式插件 extract-text-webpack-plugin
官网是这么解释的Extract text from bundle into a file.,把额外的数据加到编译好的文件中

```bash
npm install extract-text-webpack-plugin --save-dev


var ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
    module: {
        loaders: [
            {
            test: /\.css$/,
            loader: ExtractTextPlugin.extract('style-loader','css-loader!autoprefixer-loader?{browsers: ["last 1 version", "> 10%", "> 5% in US", "ie 8", "ie 7"]}')
          }
        ]
    },
    plugins: [
        new ExtractTextPlugin("style.css")
    ]
}
说明：需要手动将css标签放到index.html的body上面
```

### 自带插件
OccurenceOrderPlugin :为组件分配ID，通过这个插件webpack可以分析和优先考虑使用最多的模块，并为它们分配最小的ID
UglifyJsPlugin：压缩JS代码；
