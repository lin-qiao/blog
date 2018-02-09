---
title: 四、小程序页面逻辑
date: 2018-02-09
tags:
categories: 微信小程序
---



写在前面：微信小程序支持ES6语法，开发工具会在上传或编译的时候自动转为ES5

### 工作原理
小程序的视图层目前使用 WebView 作为渲染载体，而逻辑层是由独立的 JavascriptCore 作为运行环境。在架构上，WebView 和 JavascriptCore 都是独立的模块，并不具备数据直接共享的通道。当前，视图层和逻辑层的数据传输，实际上通过两边提供的 evaluateJavascript 所实现。即用户传输的数据，需要将其转换为字符串形式传递，同时把转换后的数据内容拼接成一份 JS 脚本，再通过执行 JS 脚本的形式传递到两边独立环境。

而 evaluateJavascript 的执行会受很多方面的影响，数据到达视图层并不是实时的。


<!-- more -->
### 修改page里边data的值

跟vue有所不同，修改data里的数据必须通过this.setData这个函数来实现。接受一个对象为参数，key为data里边对应的变量名，value为赋给它的数据

```javascript
this.setData({
  key:value
})
```

#### 数据绑定 {{}}

WXML 中的动态数据均来自对应 Page 的 data。

* 微信小程序的数据绑定是单项绑定模式，即WXML的数据发生变化，Page中data的数据并不会发生变化

* 如果数据需要写在属性内，不可以省略{{}}

微信小程序可以在 {{}} 内进行简单的运算，支持的有如下几种方式：
* 三元运算
* 算数运算
* 逻辑判断
* 字符串运算
* 数据路径运算

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/data.html

### 列表渲染 wx:for

在组件上使用 wx:for 控制属性绑定一个数组，即可使用数组中各项的数据重复渲染该组件。

* 默认数组的当前项的下标变量名默认为 index，数组当前项的变量名默认为 item
* 可以使用 `wx:for-index="idx"` ，`wx:for-item="itemName"`来改变默认值

```html
<view wx:for="{{array}}">
  {{index}}: {{item.message}}
</view>
```

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/list.html

### 条件渲染 wx:if
在框架中，使用 wx:if="{{condition}}" 来判断是否需要渲染该代码块：
```html
<block wx:if="{{true}}">
  <view> view1 </view>
  <view> view2 </view>
</block>
```
* <block/> 并不是一个组件，它仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性。

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/conditional.html

### 事件(bind和catch)

* bind事件绑定不会阻止冒泡事件向上冒泡
* catch事件绑定可以阻止冒泡事件向上冒泡

在组件中绑定一个事件处理函数。如bindtap，当用户点击该组件的时候会在该页面对应的Page中找到相应的事件处理函数。

``` html
<view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>
```

* 注意：事件不可以以`bindtap="tapName()"`传参
* 传参方式： 以data-开头，多个单词由连字符-链接，不能有大写(大写会自动转成小写)如data-element-type，最终在 event.currentTarget.dataset 中会将连字符转成驼峰elementType。
* data- 传递的参数类型可以是任意类型

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/event.html

### 请求接口 wx.request

```javascript
wx.request({
  url: 'https://URL',
  data: {},
  method: 'GET', // 请求方式
  header: {}, // 设置请求的 header
  success: function(res){
    console.log(res.data)
    // 请求成功回调
  },
  fail: function() {
    // 请求失败回调
  },
  complete: function() {
    // 请求结束回调
  }
})
```

success返回参数说明：

|参数 |	类型	| 说明	|
| --------    | :----: |:----: |
|data	|Object/String/ArrayBuffer |	开发者服务器返回的数据	|
|statusCode|	Number	|开发者服务器返回的 HTTP 状态码	|
|header|	Object	|开发者服务器返回的 HTTP Response Header|

* 如果要取服务器返回的数据，请`res.data`
* 可以用statusCode 来做一些服务器状态的提示

官方文档： https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-request.html#wxrequestobject


### 交互反馈

提示弹窗 wx.showToast()
关闭提示弹窗 wx.hideToast()
加载中弹窗 wx.showLoading()
关闭加载中弹窗 wx.hideLoading()
模态弹窗 wx.showModal()

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-react.html
