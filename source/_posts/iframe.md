---
title: iframe标签的使用
date: 2016-08-11
tags:
categories: html5
---

把iframe解释成“浏览器中的浏览器“很是恰当

```html
<iframe frameborder=0 width=170 height=100 marginheight=0 marginwidth=0 scrolling=no src=http://www.163.com></iframe>
```
iframe用于设置文本或图形的浮动图文框或容器。

<!-- more -->
### border
```html
<iframe border="3"></iframe>
```
设定围绕图文框的边缘宽度

### frameborder
```html
<iframe frameborder="0"></iframe>
```
设置边框是不否为3维（0=否，1=是）

### height,width
```html
<iframe height="31" width="88"></iframe>
```
设质边框的宽度和高度

### scrolling
```html
<iframe scrolling="no"></iframe>
```
是否有滚动条（yse,no,auto)

### src
```html
<iframe src="girl.gif"></iframe>
```
指定iframe调用的文件或图片(HTML,HTM,GIF,JPEG,JPG,PNG,TXT,*.*)
