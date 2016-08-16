---
title: jQuery滚动加载插件scrollLoading
date: 2016-08-10 17:04:35
tags:
categories: jQuery
---

常常会有这样子的页面，内容很丰富，页面很长，图片较多。这些页面图片数量多，而且比较大，少说百来K，多则上兆。要是页面载入就一次性加载完毕。所以，我们得做点什么，避免这种糟糕的状况发生。目前很流行的做法就是滚动动态加载，显示屏幕之外的图片默认是不加载的，随着页面的滚动，这个要显示图片的区域进入了浏览器可是窗口范围，则触发图片的加载显示。这种做法的好处是，一是页面加载速度快，二是节约了流量。

<!-- more -->

[点击下载scrollLoading](https://github.com/farinspace/jquery.imgpreload/blob/master/jquery.imgpreload.min.js)

## scrollLoading使用
不管怎样，首先调用jQuery库文件，还有jquery.scrollLoading.js:
```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.1/jquery.min.js"></script>
<script type="text/javascript" src="http://www.zhangxinxu.com/study/js/mini/jquery.scrollLoading-min.js"></script>
```
此插件的方法名就是scrollLoading，所以，直接：包装器.scrollLoading();就可以实现滚动加载效果了:
```javascript
$(".scrollLoading").scrollLoading();//表示所有class为scrollLoading的元素绑定了滚动加载的方法。
```


在HTML5中，以data-开头的自定义属性都是合法的，且地址可以是图片，页面等。所以，我设定了绑定地址的自定义属性为”data-url”，此属性值设为真实的图片（或页面）地址就可以了。例如下面：

```html
<div class="scrollLoading" data-url="loaded.html">加载中...</div>
```

对于常用的图片，还有一点小问题，就是其默认的src图片地址。其src地址不能是真实的图片地址（否则会直接一次性全部加载），也不能是空地址或是坏地址，否则IE浏览器下会出现很惊悚的红叉叉。做法是默认链接的是一个1px * 1px的base码，大小很小，当滚动加载的时候直接把此图片替换掉。于是，就有类似下面的代码：
```html
<img class="scrollLoading" data-url="iamges/images1.png" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsQAAA7EAZUrDhsAAAANSURBVBhXYzh8+PB/AAffA0nNPuCLAAAAAElFTkSuQmCC" width="180" height="180"/>
```
如果想让你的图片加载起来更有效果,可以这样写，或者把效果放到回调函数里边。

```javascript
$("img").load(function () {
               //图片默认隐藏  
               $(this).hide();
               //使用fadeIn特效  
               $(this).stop().fadeIn("5000");
           });
```
## scrollLoading可选参数

* attr   默认:data-url   获取元素加载地址的属性名
* container	 默认：$(window)	 滚动的容器。默认为$(window)，也就是默认的网页滚动。
* callback	 默认：$.noop	 回调。元素动态加载完毕后执行的回调函数。其中回调函数的上下文this就是当前DOM元素
