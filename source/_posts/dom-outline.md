---
title: DOM概述
date: 2016-04-09
tags:
categories: javascript
---

# DOM是什么
<!-- mdn +关键字 -->
Document Object Modal(文档对象模型)

我们在页面中看到的div,span,p,h1等等元素或文字
在javascript眼中都是一个对象

<!-- more -->

# 从一个web应用的开发说起

第一步，从页面中去选取一个元素出来
当我们的代码在浏览器中运行时，
浏览器已经帮我们创建了很多对象，
对象中有很多方法，可供我们使用，
这些东西都在一个叫做window的全局变量

window对象中的属性可以省略window.去调用

选取元素，我们从window.document开始

## 选取元素的方式

快速从Document中取出一个DOM对象的办法
* document.body
* document.head
* document.title
* document.documentEement  代表整个HTML标签的一个DOM对象
* el=document.querySelector()
* document.getElementById()

* document.querySelectorAll()
* document.getElementsByClassName()
* document.getElementsByTagName()
* document.getElementsByName()

这些方法的返回结果是什么？
前两个的返回结果代表了页面中某个元素的对象，我们把它叫做DOM对象
后四个的返回结果是一个类数组对象，我们把它叫做DOM集合。
```javascript
var el={
0:DOM对象
1:DOM对象
......
length:12;
}
```

## DOM对象中的常用属性和方法

### Object

* toString()
* valueOf()        console.log输出来的方法

### EventTarget
* addEventListener()
* removeEventListener()
* dispatchEvent()     绑定事件

### Node
所有的DOM对象都是一个节点，这三个属性用来描述节点。
* nodeName
* nodeType
* nodeValue
我们能从每个DOM对象身上取到自己相邻或父或子节点
* childNodes          DOM集合(NodeList)
* lastChild           DOM对象
* firstChild          DOM对象

* parentElement       DOM对象
* parentNode          DOM对象

* nextSibling         DOM对象  返回下一个兄弟节点
* previousSibling     DOM对象  返回上一个兄弟节点

节点的文本内容(会过滤到标签)
* textContent   

每个DOM对象中都提供了一些操作节点的方法
* appendChild()    父DOM对象.appendChild(DOM对象)     box.appendChild(el).styel.color='red';
* insertBefore()   父DOM对象.insertBefore(插入的对象，插入谁之前)    返回值 你插入的那个对象

* removeChild()                                                      返回值 移除的那个DOM对象
* replaceChild()   替换某个子节点                                    返回值 替换的那个DOM对象

* hasChildNodes()                                                    返回值 true false
* contains()     判断一个节点中包不包含另外一个节点                  返回值 true false

* cloneNode()   (true:复制所有东西 false:只复制DIV结构)             返回克隆之后的那个DOM对象





### Element
元素和节点的区别：带标签都既要元素，又是节点，不带标签的，比如DIV里边的文字，注释，它们只是节点，不是文字。

从一个DOM对象开始，获取子父兄弟元素。
* children                取一个DOM对象的子元素
* firstElementChild
* lastElementChild

* nextElementSibling     下一个元素
* previousElementSibling 上一个元素

对元素属性的操作  (HTML 元素的属性，就是头标签里的那些K=''中的K)
* classList            add remove toggle contains   
* className            可读可写
* id                   可读可写
* getAttribute()
* setAttribute()       没有返回值，只是做了一个操作
* hasAttribute()       判断元素头标签中有没有某个属性  布尔值
* removeAttribute()

获取该元素的视窗坐标或者坐标
* getBoundingClientRect()  {top:1,left:1,bottom:1,width:1,height:1}
* scrollLeft
* scrollTop

* clientWidht   一般用来结合document.documentElement.clientWidht
* clientHeight

* outerHTML
* tagName
从某个元素开始，可以缩小范围继续去查找元素。
* getElementsByClassName()
* getElementsByTagName()
* querySelector()
* querySelectorAll()

### HTMlElement
* innerHTML 可读写，能设置某个DOM对象内部的Html结构。
Warning  ：不要用这种方式追加元素 el.innerHTML+='<div></div>'
* innerText  

实时获取元素信息

* offsetHeight         返回值：数字
* offsetWidth          返回值：数字
* offsetLeft           返回值：数字
* offsetTop            返回值：数字
* offsetParent    获取具有定位属性(非static)的祖先元素。DOM对象

操作元素的行内样式
* style          可读写，读的时候实时获取元素行内样式的值，不会去计算CSS中定义的属性。52

###  HTML xxx Element
* value
* checked
* focus()
* src



# 事件
## 添加事件的两种方式及其区别

```javascript
//1 使用onxxx
var el=document.getElementById('box');
el.onclick=function(){}


//2
el.addEventListener('click',function(){},false);
```
区别：1 一些H5事件并没有onxxx这个版本
      2 onclick 再赋值一次，会覆盖上次赋值的那个函数 addEventListener 没有这个问题，它可以给一个事件添加多个函数，事件触发的
      的时候，按照添加顺序，逐个调用处理函数。
我们给DOM对象的onclick属性赋值，值为一个函数
这次赋值和普通的对象赋值不太一样。
js会告诉浏览器，密切关注这个元素，如果有人点击它
帮我把这个函数运行一下，
运行函数的时候 给我传一个参数，参数为一个对象
对象中详细的记录这次点击的一些信息，这个对象被称为事件对象。


## 自定义事件

click dblclick threeclick

``` javascript
//鼠标按住5秒的时候触发一个事件
var onlongPress=function  (el,fn) {
	var onlongpress=new Event('onlongpress');
	el.addEventListener('onlongpress',function  (e) {
		fn.call(el,e);
	})
	var timerid;
	el.addEventListener('mousedown',function  (e) {
		e.stopProPagation()
		timerid=setTimeout(function  () {
			el.dispatchEvent(onlongpress);
		},750)
	})
	el.addEventListener('mouseup',function  (e) {
		e.stopPropagation();
			clearTimeout(timerid);
		})
}


var onthreeClick=function  (el,fn) {
	var onthreeClick=new Event('onthreeClick');
	el.addEventListener('onthreeClick',function (e) {
		fn.call(el,e);
	})
	el.addEventListener('click',(function  (e) {
		e.stopPropagation();
		var i=0;
		return function  () {
			i+=1;
			if(i===3){
				el.dispatchEvent(onthreeClick);
			}
			setTimeout(function  () {
				i=0;
			},750)
		}
	})());
}
```




## 阻止事件冒泡和阻止事件默认形为
1、 从页面结构中调整，让元素之间不再是包含关系
2、 使用事件对象身上的stopPropagation()这个函数
```javascript
e.stopPropagation()
```
## 阻止事件的默认行为
要从事件的根源去阻止默认行为
```javascript
e.preventDefault()
```
# 函数
## 函数重载
同一个函数因为参数类型或数量的不同，可以对应多个函数的实现，每种实现对应一个函数体。

## 回调函数
当我们把函数X做为参数传给函数Y，在函数Y内部有对函数X的调用，我们把函数X叫做回调函数。

## 递归函数
在函数内部直接或者间接调用自己

## 闭包
在函数内再嵌套函数

# 数组
## 遍历
如果就是数组，我们遍历的时候可以使用
```javascript
var arr=[1,3,4,5];
arr.forEach(function(v){console.log(v)})
```
如果是类数组，我们遍历的时候可以使用

```javascript
var arr=[],
forEach=arr.forEach,
filter=arr.filter;
var els=document.querySelectorAll('div')

forEach.call(els,function(v){console.log(v)})
```
