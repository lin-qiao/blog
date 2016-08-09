---
title: jquery概述
date: 2016-08-07 17:04:35
tags:
categories: jQuery
---

前端开发在开始写代码之前，一般首先要解决的两个问题
* 1.解决js标准本身的兼容性问题 es5shim.js
* 2.解决DOM扩展部分的兼容性问题 jQuery

<!-- more -->

jQuery 是一个javascript库
库可以简单理解成一堆以某种方式组织起来的，方便，易用的函数的集合。


## jQuery的优点

1.全面解决PC端的兼容性问题
2.语法精炼，性能好，插件库非常庞大。

## jQuery版本号之间的区别

1.xxx->1.12;  支持ie8
2.0;          彻底的放弃了对ie<10的支持，转向移动端。


## jQuery原理
1.new 的时候究竟发生了什么
  * 构建一个空对象{}
  * mao.call({})把mao 那个函数作为这个空对象的临时方法调用一次
  * 把mao 那个函数对象身上的prototype那个属性拿来,作为自己原型链中的一条
  * 返回最终的对象
2.对象的原形链
3.函数对象身上一个属性(prototype)和一个方法(call)
4.this的指向
  以jQuery函数为原型构造的那个对象

```javascript
(function(){
    var jQuery=function(selector){
        var els=document.querySelectorAll(selector);
        for( var i = 0 ; i < els.length ; i ++){
            this[i]=els[i];
        }
         this.length=els.length;
    }
	jQuery.prototype.addClass=function(str){
        for ( var i =0 ; i < this.length ; i ++){
            this[i].classList.add(str);
        }
    }
	var $=function(selector){
        return new jQuery(selector)
	}
	window.$=$;
})()
```
## jQuery 中大多数方法都会返回一个jQuery集合;
* 操作集合的方法返回的就是原集合
* 对集合做过过滤或者导致集合改变的一些方法返回改变后的jQuery集合
* $ .append 这一类方法，当涉及到创建DOM对象时，他们会返回创建完成后的一个jQuery集合
所以在jQuery中链式调用很常见
$函数能接受的参数类型以及对应的返回值

```javascript
$('#selectors h1')
.width(100)
.height(100)
.css('color','red')
.position(); //链条在这里断
```

* null      jQuery对象
* 数组,对象  处理过的jQuery对象
* 选择器
* html标签
* DOM对象
* DOM集合
* 函数

##jQuery中的方法
jQuery中的方法分两类：
1.直接出现在jQuery函数对象身上，是一些基础性质的工具函数
2.出现在jQuery.fn函数对象的原型链上，用来批量操作jQuery集合中的dom元素

大部分方法重载很严重
```javascript
$('li').css('width') //取出选中集合中第一个元素的宽度+px
$('li').css('width') //设置集合中的所有元素宽度为200px
$('li').css({width:200,height:300,border:'1px solid black'}) //给选中集合中的所有元素加上传入对象的所有CSS样式
$('li').css('width',function(){
  return Math.random();
})
//给集合中的每个元素添加'width'这个属性,每一个元素的width属性的值由第二个参数传入的函数来计算,也就意味着，jQuery会对集合中的每一个元素调用用户传入的回调函数 调用的时候会传入两个参数，一个是当前元素在集合的位置(0,1,2,3)，另外一个是当前元素现有的'width'值
$('li').css(
  width:function(){return Math.random},
  height:function(){return Math.random},
  backgroundColor:function(i){return colors[i]}
)

jQuery库设计理念
1.解决兼容性问题
2.让从页面中查找元素变得更轻松
3.提供很多内置方法 使对dom对象的操作变得更轻松

css
addClass  这些方法对过内置的遍历去操作每一个dom元素
jQuery不希望我们在循环中使用这些内置方法
1.给集合中的每一个dom元素设置同样的值或行为
2.给集合中的每一个dom元素设置不同的值或行为

```javascript
var els=document.querySelectorAll('.item');
for(var i = 0; i < els.length; i++){
  $(els[i]).addClass('aa')
}
```
## 给jquery添加一个字符串前后换位置的函数
```javascript
$.extend({'reversestring':function(s){
       return s.split('').reverse().join('')
}})
```


## jQuery插件
1、引入jQuery
2、引入插件 jquery.插件名.js
3、$('.lunbo').插件名();

(function($){
  var 插件函数=function(){
   return this;
  }
  $.fn.extend({插件名:插件函数})
})(jQuery)
```html
第一个引入的js库用来解决兼容性问题
<script src="jquery.js"></script>
<script src="header.js"></script>
<script src='footer.js'></script>
```
分开引用的js文件一定要解决的一个问题
不能污染全局变量

```javascript
var getElement=function(selector){
	if(document.querySelector){
	return document.querySelector(selector)
   }else{
      console.error('请更换浏览器')
      console.warnning('浏览器不支持')
   }
}
```

上面这种写法的两个问题
1.用户需要学习一门全新的语法
2.用户需要避开那个库中的所有全局变量


  //new 关键字来构造对象
  var mao=function  () {
  	this.zhuanzi=4;
  	this.erduo=1;
  }
  每一个函数对象身上，都有一个其他对象不具备的属性 叫做'prototype'
  var bosimao=new mao();

  1.构建一个空对象{}
  2.mao.call({})把mao 那个函数作为这个空对象的临时方法调用一次
  3.把mao 那个函数对象身上的prototype那个属性拿来，
  作为自己原型链中的一条
  4.返回最终的对象


  ## 实现原理
  DOM对象
  ```javascript
  var el={
    offsetWidth:12,
    _proto_:(HTMLDivElement)
    title:'aa'
     _proto_:HTMLElement
      src:'xxx.png'
      _proto_:Element
       getAttribute:fn
       _proto_:node
       nodename:'IMG'
        _proto_:Object
  }
  ```
  DOM集合

  ```javascript
  var els={
    0:div,
    1:div,
    2:div,
    length:3,
    _proto_:(并没有多少有价值的方法或属性)
  }
  ```
  jQuery对象

```javascript
var jQuery={
	0:div,
	1:div,
	2:div,
	length:3,
	_proto_
	   addClass:fn,
	   removeClass:fn,
	   css:fn,
	   prop:fn,
	   ...
}
```

##jQuery重载方式
* $.attr('aa')
* $.attr('aa','1')
* $.attr('aa',function(){return Math.random()})
* $.attr({'aa':'1','bb':'2'})
* $.attr({'aa':function(){return Math.random()},'bb':2})
* 以空格分割开的字符串


## jQuery中能使用的选择器
jQuery中的选择器使用一个叫sizzle.js的库
.a p > a .item  从后往前找(在树中，找父元素只是一次查找，找子元素需要遍历)
* parent > child 子选择器 父元素下的直接子元素
* A B            后代选择器
* prev + next    下一个选择器
* prev ~ next    下一个所有选择器

在找到的集合中筛选 find()
* :animatd        正在执行jQuery动画的元素
* :eq()           下标等于几的元素 从0开始
* :gt             大于
* :lt             小于
* :first
* :last
* :even           偶数
* :odd            奇数
* :header         h1-h6标签 $(this).find('*:header')
* :lang()         找属性中有lang的元素 $(this).find('div:lang(spannishi)')
* :target         index.html#A
* :root           找出html
* :not()          接受简单选择器，选择不是not的元素            

* :contains()    过滤内容中包含某个字符的元素
* :empty         $('p:empty') 没有内容的
* :has()         括号中可以接受一个简单选择器(类名 标签 ID) 判断子元素里有没有
* :parent        选中元素中包含子元素的

* :visible       选中元素中可见的元素
* :hidden        先中元素中不可见的元素


* [attr]         从已选中元素筛选有这个属性的
* [attr=aa]      精确等于aa
* [attr!=aa]     
* [attr|=aa]     aa开头或aa-开头
* [attr*=aa]     包含aa
* [attr^=aa]     以aa为开头
* [attr~=aa]     class="xx aa zz"  空格隔开的多个值 里面有aa这个属性值
* [attr$=aa]     以aa为结尾的


* :first-child      保留是父元素第1个的元素   可以用2n+1 取奇数 2n取偶数
* :last-child       保留是父元素最后1个的元素
* :nth-child()      保留是父元素第N个的元素
* :nth-last-child() 保留是父元素倒数第N个的元素

* :nth-of-type()      保留是父元素下面同类标签的第N个元素
* :nth-last-of-type() 保留是父元素下面同类标签的倒数第N个元素
* :first-of-type      保留是父元素下面同类标签的第1个元素
* :last-of-type       保留是父元素下面同类标签的最后1个元素

* :only-child         保留是父元素下面唯一一个子元素的元素
* :only-of-type       保留是父元素下面同类标签唯一一个子元素的元素

* :disabled           可以把表单设置为不可输入状态
* :enabled    


* attr()  可读可写  设置 取值
* removeAttr() 删除
* prop()    表单属性
* val()    value属性


* css()   接收一个对象{background:red,height:100}

* height()
* innerHeight()    height+padding
* outerHeight()    innerHeight()+border
* outerHeight(true)  outerHeight()+margin


* offset()            不会被定位属性拦截
* offsetParent()     找具有定位属性的父元素
* position()   offsetTop offsetLeft

.clone()    拷贝后得到对象父元素信息会丢失

给jQuery对象中的元素添加子元素(参数可以是dom对象 dom集合 html规则的字符串 回调函数 jQuery对象  返回值：谁调用的返回谁)
.append()       添加到最后
.prepend()      添加到最前

给jQuery对象的中元素添加兄弟元素
.after()        添加到自己之后
.before()       添加到自己之前

把jQuery对象作为子元素添加到某个元素中去(参数可以是dom dom对象 jQuery对象 选择器
返回值：谁调用的返回谁)
.appendTo()
.prependTo()

把jQuery对象作为兄弟元素添加到某个元素之前之后
.insertBefore()
.insertAfter()


.wrap()   把每个jQuery对象用一个东西包起来
.wrapAll() 把所有jQuery对象包到一个东西里
.wrapInner() 把jQuery对象里的内容用一个东西包起来

.remove()   移除某个jQuery对象 连带事件
.detach()   移除某个jQuery对象 不会移除事件
.unwrap()   移除某个jQuery对象的父元素标签
.empty()    移除某个jQuery对象中的全部内容


.replaceWith()    把某个jQuery对象替换成一个字符串
.replaceAll()     用某个东西替换一个参数，参数为一个选择器

eq()

.filter()      找出所有jQuery 通过过滤找出符合条件的 授受一个选择器 jQuery对象 dom对象 dom集合 回调函数
.has()         有没有
.is()          是不要某个标签， 接受一个简单选择器
.not()         找出jQuery没有某个属性的元素
.slice(x,x)    找出jQuery中下标为x-(x-1)的所有元素 如果x为负数，为倒数
.map()         遍历jQuery对象,参数为一个函数 有两个参数 i v 有reurn 会返回一个数组，没有返回一个空数组
.add()          合并两个集合  自动排序  去重
.contents()     找出所有节点

.end()         链式调用

.addBack()   找到它下面子元素并且把自己放到数组里
.children()   接受选择器作为参数
.find()       找自己子元素里满足条件的元素
.parents()    找某个元素所有的祖先元素,接受一个选择器过滤
.closest()    传递一个选择器, 找你最近的一个满足条件的元素

.nextAll()    可以接受一个选择器，找出下面所有满足条件的元素
.nextUntil()   接受一个选择器，找下面直到某个标签满足条件
.siblings()    找出了除自己以外的所有满足条件的兄弟元素



on  添加事件的方法
```javascript
1.$(.'item').on('click',function(){})

2.var clickfn=function(){console.log(1)}
$('.item').on('click mouseleave mouseenter',clickfn)

3.
var fn1=function(){}
var fn2=function(){}
var fn3=function(){}
$('.item').on({'click':fn1,'mouseleave':fn2,'mouseenter':fn3})

4.事件委托
$('.item').on('click','li',function(){})
//注意
$('.item').delegate('li','click',function(){})
```

.one()   绑定一个事件，这个事件只能运行一次
.trigger() 逼真模仿浏览器触发一次(冒泡行为)
.triggerHandler() 不会冒泡 模仿浏览器触一次

.mouseout
.mouseover   会冒泡

.mouseenter() 不会冒泡
.mouseleave()

.focus  冒泡
.focusin 不会冒泡
.bind()    addEventListener    函数里return false,  阻止事件冒泡和默认行为

jQuery事件总结

1.on()的几种添加方式
  * 一次绑定多个事件 用同一个函数处理
  * 一次绑定多个事件 用不同的函数处理
2.事件委托的添加方式(只能使用on和delegate方法)
3.mouseover mouseout 方法由于事件冒泡带来的多次触发,一般我们使用mouseenter和mouseleave以及hover(hover是对mouseenter和mouseleave的封装)
4.focus blur ->focusin focusout 同上后面两个不会冒泡
5.ready()事件一般用$(function(){})来替代
  内部的处理方式是document.addEventListener('DOMcontentLoaded');
  如果文档结构(不包含图片,脚本等)已经加载完成，直接运行函数
  如果还没有加载完成,把函数绑定到'DOMContentLoaded'事件
6.trigger() 会模仿浏览器触发一次事件(会冒泡) triggerHandler 不会
7.所有事件添加函数的返回值都是要绑定事件的那个jQuery对象，所以可以继续链式调用,例如$('.item').click(function(){console.log(1)}).css();

jQuery中的事件对象
继承自原生的事件对象  e.keyCode e.altKey e.target e.clientX

* e.currentTarget

hide(时间,方式,回调函数)

## 动画
.stop()      停掉当前动画 执行下一个动画
.stop(true)  停掉当前动画,清除所有队列
.stop(true,true) 到当前动画的最终状态,清除所有队列
.finish()    停掉当前动画，到当前动画最终状态
.clearQueue() 队列中所有尚未执行的函数都将被清空

.delay(1000)  延迟1秒
.queue   入队
.dequeue() 出队
$.fx.off  用于设置或返回是否全局性地禁用所有动画。true 禁用 false 启用
$.fx.interval 用于设置jQuery动画每隔多少毫秒绘制一帧图像(触发一次样式更改，浏览器可能会重新绘制当前页面)

$(':animated').finish();把所有正在动画的元素停掉
$(this).is(':animated')  


### 同步
 * 正常的函数
 * 正常的赋值
 * 正常的逻辑运算，普通运算

### 异步:
 * 事件
 * 动画 (setInterval setTimeout)
 * ajax (发送网络请求)


$.unique
$.trim  去字符串两边空格
$.proxy  改this
$.parseXML 数据交换，是一种数据通信格式  $($.parseXML('<ab><cd>11</cd></ab>')).find('cd').text()
$.parseJSON   各个语言交换数据的一种格式
$.parseHTML   把字符串转换成HTMl语言
$.now()       
$.noop()     空函数
$.merge()    把两个存储结构类数组的集合合并，经常用做参数合并
$.map()      遍历一个类数组 ，生成一个新的数组 $.map([1,2,3,4],function(){return v*2})
$.makeArray()  把类数组对象转换为一个数组
$.isPlainObject()  检测是不是一个纯粹的对象
$.isNumeric()      检测是不是一个数字   NaN不是一个数字
$.inArray()        函数用于在类数组中搜索指定的值，并返回其索引值。第一个参数不为jQuery对象 如果数组中不存在该值，则返回 -1。
$.grep()          filter
$.globalEval()     
$.each()          遍历任何东西
$.contains()      用于判断另一个DOM元素是否是指定DOM元素的后代。第一个参数为祖先元素，第二个参数为可能包含的后代元素


.ajaxStart(fn)  开始
.ajaxSend(fn)   发送
.ajaxSuccess(fn) 成功
.ajaxError()     失败
.ajaxComplete()  完成
.ajaxStop()      停止
