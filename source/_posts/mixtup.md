---
title: jQuery插件MixItUp实现动画过滤和排序
date: 2016-08-17
tags:
categories: jQuery
---

## 什么是MixItUp？

MixItUp是一个jQuery插件，提供动画过滤和排序。
MixItUp是伟大的，像管理投资组合，画廊和博客的任何分类或排序的内容，而且还可以作为一个功能强大的工具，从事应用程序的用户界面和数据可视化。
MixItUp起着很好的与您现有的HTML和CSS，使之成为响应布局的绝佳选择。
而不是用绝对定位来控制布局，MixItUp设计与现有的在线流布局工作。要使用百分比，媒体查询，inline-block的，甚至是弯曲盒子？没问题！

<!-- more -->

[实例](https://mixitup.kunkalabs.com/)

[点击下载MixItUp](http://www.bootcdn.cn/mixitup/)


## 页面代码

```html
<div id="Container">
  <div class="mix category-1" data-my-order="1"> 1 </div>
  <div class="mix category-1" data-my-order="2"> 2 </div>
  <div class="mix category-2" data-my-order="3"> 3 </div>
  <div class="mix category-2" data-my-order="4"> 4 </div>
</div>
```
* MixItUp目标与类混合容器中的元素。分类过滤添加为类和排序属性添加自定义数据属性。

> 建立过滤器控制：

```html
<div class="filter" data-filter=".category-1">Category 1</div>
<div class="filter" data-filter=".category-2">Category 2</div>
```
* 过滤发生在过滤器按钮被点击。

> 建立排序控件：

```html
<div class="sort" data-sort="my-order:asc">Ascending Order </div>
<div class="sort" data-sort="my-order:desc">Descending Order</div>
```
* 排序发生在排序按钮被点击。

## css

```css
#Container .mix{
  display: none;
}
```
* 在项目的样式，设置目标元素没有显示属性。 MixItUp将只显示那些匹配当前的过滤元件。

>  点击是会给过滤器加一个active类，可以修改你喜欢的样式

```css
.fliter.active{
  background: #344B59;
  color: white;
  border-top: 1px solid #344B59;
  border-bottom: 0 none;
}
```
## js

```javascript
$(function(){
  $('#Container').mixItUp();  
});
```
