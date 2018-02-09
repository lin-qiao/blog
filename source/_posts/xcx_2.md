---
title: 二、小程序基本文件结构
date: 2018-02-09
tags:
categories: 微信小程序
---


#### 1、小程序的文件结构
小程序包含一个描述整体程序的 app 和 多个描述各自页面的 page。一个小程序主体部分由三个文件组成，必须放在项目的根目录，如下：

| 文件        | 必填    |  说明  |
| --------    | -----: | :----: |
| app.js        | 是      |   小程序的主逻辑和全部变量    |
| app.json        | 是      |   小程序的公共设置    |
| app.wxss        | 是      |   小程序的公共样式表    |

<!-- more -->

| 文件类型        | 必填    |  说明  |
| --------    | -----: | :----: |
| js      | 是      |   页面逻辑    |
| wxml        | 是      |   页面结构    |
| wxss        | 否      |   页面样式表    |
| json        | 否      |   页面配置    |

提醒：为了方便开发者减少配置项，规定描述页面的四个文件必须具有相同的路径与文件名。编译时，将自动关联。

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/structure.html

#### 2、app.js
App() 函数用来注册一个小程序。接受一个 object 参数，其指定小程序的生命周期函数等。

生命周期如下：

|属性	|类型	|描述	|触发时机|
| --------    | -----: | :----: | :----: |
|onLaunch|	Function	|生命周期函数--监听小程序初始化	|当小程序初始化完成时，会触发 onLaunch（全局只触发一次）|
|onShow	|Function|	生命周期函数--监听小程序显示	|当小程序启动，或从后台进入前台显示，会触发 onShow|
|onHide	|Function|	生命周期函数--监听小程序隐藏	|当小程序从前台进入后台，会触发 onHide|
|onError	|Function	|错误监听函数	|当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息|
|其他	|Any	|	|开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问|

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/app-service/app.html

#### app.json
app.json文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置底部 tab 等。


|属性	|类型	|必填	|描述|
| --------    | -----: | :----: | :----: |
|pages	|String Array	|是	|设置页面路径|
|window	|Object	|否	|设置默认页面的窗口表现|
|tabBar	|Object	|否	|设置底部 tab 的表现|
|networkTimeout|	Object	|否	|设置网络超时时间|
|debug	|Boolean|	否	|设置是否开启 debug 模式|

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/config.html

#### 页面（index.wxml）
用来存放小程序页面的主要架构,类html

#### 页面样式（index.wxss）
用来存放小程序页面的样式，类css
官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxss.html

#### 页面逻辑（index.js）
Page() 函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等。

生命周期如下：

|属性|	类型	|描述|
| --------    | -----: | :----: |
| data	|Object	|页面的初始数据|
| onLoad	|Function	|生命周期函数--监听页面加载|
| onReady	|Function	|生命周期函数--监听页面初次渲染完成|
| onShow	|Function	|生命周期函数--监听页面显示|
| onHide	|Function	|生命周期函数--监听页面隐藏|
| onUnload	|Function	|生命周期函数--监听页面卸载|
| onPullDownRefresh|	Function	|页面相关事件处理函数--监听用户下拉动作|
| onReachBottom|	Function	|页面上拉触底事件的处理函数|
| onShareAppMessage|	Function	|用户点击右上角转发|
| onPageScroll	|Function	|页面滚动触发事件的处理函数|
| onTabItemTap	|Function	|当前是 tab 页时，点击 tab 时触发|
| 其他	|Any	|开发者可以添加任意的函数或数据到 object 参数中，在页面的函数中用 this 可以访问|

官方文档：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/app-service/page.html
