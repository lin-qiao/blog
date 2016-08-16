---
title: 网页中对字体段落操作的一些技巧
date: 2016-08-05 17:04:35
tags:
categories: css
---

常常看见网页中一段字体右边凹凸不平,看起来实在是不美观，翻阅了大量的网页，总结如下:

<!-- more -->

## 如何透过CSS控制文字位置

```CSS
body{
  text-align: justify;  /*两端对齐*/
  word-break: break-all;  /*控制标点不在第一个字显示*/
}
```
## 开头空两个字符并两端对齐

```css
body{
  text-indent: 2em;
  word-break: break-all;
}
```
## 实现div两端对齐
```css
div{
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: justify;
  -webkit-justify-content: space-between;
  -ms-flex-pack: justify;
  justify-content: space-between;
}
```
## 控制文字在一行并且出现...
```css
div{
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```
## 控制文字在多行显示并且出现...
```css
div{
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}
```
