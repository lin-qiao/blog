---
title: 三、小程序页面切图
date: 2018-02-09
tags:
categories: 微信小程序
---


###  wxml
wxml 常用的有view、image、text、navigator、button 等组件

#### a、view 组件 等价于 div，唯一作用是：容器、分割元素。
```html
<view class="container"></view>
```
官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/view.html

<!-- more -->
#### b、image 组件 的 src 指定图片的路径

```html
<image src="/imgs/minat.jpg" />
```
* 注：image组件默认宽度300px、高度225px
* mode 属性是图片裁剪、缩放的模式，共有13种模式，其中 4 种是缩放模式，9 种是裁剪模式。其中引用最多的是widthFix

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/image.html

#### c、text 组件显示文本，可以直接使用文本

```html
<text>文本</text>
```
* 若移动端需要文本可以长按选中的话，必须使用 text 组件
* text 组件中仅可嵌套 text 组件，其他组件会被编译器自动移除

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/text.html

#### d、navigator组件,进行程序内跳转，可以携带参数

```html
<navigator url="/page/navigate/navigate?title=navigate">跳转到新页面</navigator>
```

navigator有五种跳转方式，在属性open-type中进行配置，另外也可以通过api(wx.navigateTo)等方式进行跳转：

| 参数        | 描述    |  说明  |
| --------    | -----: | :----: |
|navigateTo	| 新窗口打开页面    |  类：vue(this.$route.push)|
|redirectTo	| 关闭当前页面，并打开新页面 | 类：vue(this.$route.replace)|
|switchTab	| 切换到 tabbar 页面   |不可以携带参数|
|navigateBack	| 退回上一个页面   |类：vue(this.$route.go(-1))|
|reLaunch| 关闭所有页面，打开到应用内的某个页面。|  | |

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/navigator.html

#### d、button 按钮组件
```html
  <button form-type="submit">点击提交表单</button>
```
* form-type 用于 <form/> 组件，点击分别会触发 <form/> 组件的 submit/reset 事件 有效值 submit/reset

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/button.html

#### e、input 组件

```html
 <input placeholder="输入框" />
```
* type 属性

|值	|说明|
| --------    | :----: |
|text	|文本输入键盘|
|number	|数字输入键盘|
|idcard	|身份证输入键盘|
|digit|	带小数点的数字键盘|

* password	是否是密码类

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/input.html

#### f、form 组件，用于将组件内的用户输入的<switch/> <input/> <checkbox/> <slider/> <radio/> <picker/> 提交。

```html
<form bindsubmit="formSubmit" bindreset="formReset">
  <input placeholder="输入框" name="key"/>
</form>
```
* 当点击 <form/> 表单中 formType 为 submit 的 <button/> 组件时，会将表单组件中的 value 值进行提交，需要在表单组件中加上 name 来作为 key。
* `bindsubmit` 	携带 form 中的数据触发 submit 事件，event.detail = {value : {'name': 'value'} , formId: ''}
* `bindreset`   表单重置时会触发 reset 事件

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/form.html

### wxss

单位：rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。其他跟css大致相同

* background 不能使用本地图片作为背景图片，但可以使用线上图片

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxss.html
