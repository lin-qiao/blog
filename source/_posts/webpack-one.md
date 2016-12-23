---
title: webpack基础
date: 2016-11-30
tags:
categories: webpack
---

## 什么是Webpack？
事实上它是一个打包工具，而不是像RequireJS或SeaJS这样的模块加载器，通过使用Webpack，能够像Node.js一样处理依赖关系，然后解析出模块之间的依赖，将代码打包

<!-- more -->
## 安装Webpack
首先得有Node.js

然后通过`npm install -g webpack`安装webpack，当然也可以通过gulp来处理webpack任务，如果使用gulp的话就`npm install --save-dev gulp-webpack`

## 配置Webpack
Webpack的构建过程需要一个配置文件，一个典型的配置文件大概就是这样
```bash
var webpack = require('webpack');
var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');

module.exports = {
    entry: {
        entry1: './entry/entry1.js',
        entry2: './entry/entry2.js'
    },
    output: {
        path: __dirname,
        filename: '[name].entry.js'
    },
    resolve: {
        extensions: ['', '.js', '.jsx']
    },
    module: {
        loaders: [{
            test: /\.js$/,
            loader: 'babel-loader'
        }, {
            test: /\.jsx$/,
            loader: 'babel-loader!jsx-loader?harmony'
        }]
    },
    plugins: [commonsPlugin]
};
```
这里对Webpack的打包行为做了配置，主要分为几个部分：

* entry：指定打包的入口文件，每有一个键值对，就是一个入口文件
* output：配置打包结果，path定义了输出的文件夹，filename则定义了打包结果文件的名称，filename里面的[name]会由entry中的键（这里是entry1和entry2）替换
* resolve：定义了解析模块路径时的配置，常用的就是extensions，可以用来指定模块的后缀，这样在引入模块时就不需要写后缀了，会自动补全
* module：定义了对模块的处理逻辑，这里可以用loaders定义了一系列的加载器，以及一些正则。当需要加载的文件匹配test的正则时，就会调用后面的loader对文件进行处理，这正是webpack强大的原因。比如这里定义了凡是.js结尾的文件都是用babel-loader做处理，而.jsx结尾的文件会先经过jsx-loader处理，然后经过babel-loader处理。当然这些loader也需要通过npm install安装
* plugins: 这里定义了需要使用的插件，比如commonsPlugin在打包多个入口文件时会提取出公用的部分，生成common.js
当然Webpack还有很多其他的配置，具体可以参照它的[配置文档](http://webpack.github.io/docs/configuration.html#entry)

## 执行打包
如果通过`npm install -g webpack`方式安装webpack的话，可以通过命令行直接执行打包命令，比如这样：
```bash
$webpack --config webpack.config.js
```
## 组件编写
### 使用Babel提升逼格
```bash
Webpack使得我们可以使用Node.js的CommonJS规范来编写模块，比如一个简单的Hello world模块，就可以这么处理：

var React = require('react');

var HelloWorldComponent = React.createClass({
    displayName: 'HelloWorldComponent',
    render: function() {
        return (

<div>Hello world</div>

);
    }
});

module.exports = HelloWorldComponent;
```
等等，这和之前的写法没啥差别啊，依旧没有逼格...程序员敲码要有geek范，要逼格than逼格，这太low了。现在都ES6了，React的代码也要写ES6，`babel-loader`就是干这个的。Babel能够将ES6代码转换成ES5。首先需要通过命令npm `install --save-dev babel-loader`来进行安装，安装完成后就可以使用了，一种使用方式是之前介绍的在`webpack.config.js`的loaders中配置，另一种是直接在代码中使用，比如：
```bash
var HelloWorldComponent = require('!babel!jsx!./HelloWorldComponent');
```
那我们应当如何使用Babel提升代码的逼格呢？改造一下之前的HelloWorld代码吧：
```bash
import React from 'react';

export default class HelloWorldComponent extends React.Component {
    constructor() {
        super();
        this.state = {};
    }
    render() {
        return (

<div>Hello World</div>

);
    }
}
```
这样在其他组件中需要引入HelloWorldComponent组件，就只要就可以了：
```bash
import HelloWorldComponent from './HelloWorldComponent'
```
怎么样是不是更有逼格了？通过import引入模块，还可以直接定义类和类的继承关系，这里也不再需要getInitialState了，直接在构造函数constructor中用this.state = xxx就好了

Babel带来的当然还不止这些，在其帮助下还能尝试很多优秀的ES6特性，比如箭头函数，箭头函数的特点就是内部的this和外部保持一致，从此可以和that、this说再见了
```bash
['H', 'e', 'l', 'l', 'o'].map((c) => {
    return (<span>{c}</span>);
});
```


### 样式编写

没错，依旧是使用loader

可以在webpack.config.js的loaders中增加sass的配置：
```bash
{
  test: /\.scss$/,
  loader: 'style-loader!css-loader!autoprefixer-loader!sass-loader'
}
```
通过这样的配置，就可以直接在模块代码中引入sass样式了：
```bash
import React from 'react';

require('./style.scss');

export default class HelloWorldComponent extends React.Component {
    constructor() {
        super();
        this.state = {};
    }
    render() {
        return (

<div>Hello World</div>

);
    }
}
```
## 其他

Webpack的loader为React组件化提供了很多帮助，像图片也提供了相关的loader：
```bash
{
  test: /\.(png|jpg|gif|woff|woff2)$/,
  loader: 'url-loader?limit=8192'
}
# limit=8192 图片小于8K时自动转换为base64码
```
更多地loader可以移步[webpack的wiki](https://github.com/webpack/docs/wiki/list-of-loaders)
