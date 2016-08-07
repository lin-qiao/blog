---
title: canvas概述
date: 2016-08-07 17:04:35
tags:
categories: html5
---

canvas画布的特点：绘制上的元素无法更改

<!-- more -->

## 形状

>矩形

* ctx.fillRect()      填充矩形
* ctx.strokeRect()    框线矩形

```javascript
ctx.fillRect(150,30,300,100);//(X,Y,W,H)
ctx.strokeRect(150,30,300,100);//(x,y,w,h)
```
>圆

* ctx.arc()        圆

```javascript
ctx.arc(150,300,50,0,(Math.PI/180)*360,true);
//(x,y,半径,起始角度,最后角度,方向(true为逆时针))
ctx.fill();
```
> 线

* ctx.beginPath()开始一个路径
* ctx.moveTo(1,1)移动画笔某点
* ctx.lineTo(1,1)让路径中`拥有`(不会直接绘出)一条到某点的线
* ctx.rect()让路径中`拥有`(不会直接绘出)一条矩形
* ctx.fill()        填充当前路径
* ctx.stroke()      描边当前路径
* ctx.closePath()   结束一个路径

>曲线

* ctx.quadraticCurveTo(cp1x,cp1y,x,y) 二次贝塞尔
* ctx.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y) 三次贝塞尔


## 从画布中清除一个区域(两种方式)

* ctx.clearRect(0,0,600,600)   
* canvas.width=cavas.height = 600;

## 位移(挪动画布)
> 只保存画布的状态(包括画笔)`状态` 不保存任何图像

* ctx.save()
* ctx.restore()
* ctx.translate()
* ctx.rotate( (Math.PI/180) * 30);
* ctx.scale();

## 样式

* ctx.fillStyle='rgba()'' 边框颜色
* ctx.strokeStrye='#1b1111'背景颜色
* ctx.globalAlpha='0.2';    透明度
* ctx.lineWidth             线的粗细
* ctx.lineCap

>线段端点显示的样子butt，round 和 square。默认是 butt。

* ctx.lineJoin

>lineJoin 的属性值决定了图形中两线段连接处所显示的样子。它可以是这三种之一：round, bevel 和 miter。默认是 miter。

* ctx.miterlimit
* ctx.getLineDash()
* ctx.setLineDash([10,5])  制定虚线样式
* ctx.lineDashOffset=5 设置起始偏移量.

* createLinearGradient(x1, y1, x2, y2)
表示渐变的起点 (x1,y1) 与终点 (x2,y2)。
* createRadialGradient(x1, y1, r1, x2, y2, r2)
前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。
* addColorStop(position, color)
position 参数必须是一个 0.0 与 1.0 之间的数值color 参数必须是一个有效的 CSS 颜色值

## 使用背景图片的两种方式
```javascript
window.onload=function () {
  var img=document.querySelector('img');
  var xxx=ctx.createPattern(img,'no-repeat');
  ctx.fillStyle =xxx;
}
```
```javascript
var img=new Image;
img.src='xxx.jpg';
img.onload=function () {
  var xxx=ctx.createPattern(img,'no-repeat');
  ctx.fillStyle=xxx;
}
```
##阴影 Shadows
* shadowOffsetX = float

>shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。

* shadowOffsetY = float

>shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。

* shadowBlur = float

> shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。

* shadowColor = color

>shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。


## 绘制文本
* fillText(text,x,y)
* strokeText(text,x,y)

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  ctx.font = "48px serif";
  ctx.fillText("Hello world", 10, 50);
}
```

## 绘制图片

* drawImage(image,x,y)

>其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。

## 缩放 Scaling
* drawImage(image, x, y, width, height)

>这个方法多了2个参数：width 和 height，这两个参数用来控制 当像canvas画入时应该缩放的大小

## 切片 Slicing
* drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

>第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照右边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。

```javascript
function draw() {
  var canvas = document.getElementById('canvas');
  var ctx = canvas.getContext('2d');

  // Draw slice
  ctx.drawImage(document.getElementById('source'),
                33,71,104,124,21,20,87,104);

  // Draw frame
  ctx.drawImage(document.getElementById('frame'),0,0);
}
```
