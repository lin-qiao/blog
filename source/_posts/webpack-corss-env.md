---
title: webpack设置环境变量来区分生产和测试环境
date: 2019-03-28
tags:
categories: webpack
---
每次打包前都需要修改相应环境的baseUrl, 感觉非常麻烦

test环境：请求baseUrl = 'http://www.ddtkj.cn/';
pro环境：请求baseUrl = 'http://www.dadetong.com/';
<!-- more -->

### 解决方案
webpack环境变量 和 DefinePlugin插件

> DefinePlugin这个插件可以定义一个global的变量，这个变量可以在业务就是js代码中（也就是src的js中）的任何文件被访问到。这样我们就可以轻松的区分各种环境啦。

#### 1. 话不多说，第一步就是安装必要的插件

`npm i --save-dev cross-env`

#### 2.修改package.json 文件
添加build:test，并给它设置环境变量为testing，此时webpack文件已经可以取到所设置的NODE_ENV。

```json
"scripts": {
  "start": "cross-env NODE_ENV=testing node build/dev-server.js",
  "reload": "node build/dev-server.js",
  "build:test":"cross-env NODE_ENV=testing node build/build.js",
  "build": "node build/build.js"
},
```
> Webpack是属于Node的程序，Node环境下的环境变量，Webpack可以配置可以灵活读取

#### 3.修改webpackage.prod.conf.js 文件
src的js文件属于Webpack要构建的产物，如果里面也想读取环境变量。可以通过DefinePlugin来完成

```javascript
plugins: [
    new webpack.DefinePlugin({
        // 'process.env': env,
        'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
    }),
  ]
```
> 现在js文件也可用通过`process.env.NODE_ENV`取到所设置的环境变量了

#### 4.使用不用的baseUrl

```javascript
let baseUrl = 'http://www.dadetong.com/';
if(process.env.NODE_ENV == 'testing'){
  baseUrl = 'http://www.ddtkj.cn/';
}
```

到此就大功告成啦！！！
