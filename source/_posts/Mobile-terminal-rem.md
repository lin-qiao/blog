---
title: 用sass自动编写适合各个屏幕尺寸的html基准字体
date: 2016-08-06 17:04:35
tags:
categories: 移动端
---

接收一个设计图的尺寸，帮我们生成对应的html的字体大小，为了方便编写CSS样式的rem值，在这里我习惯采用html字体为100opx

<!-- more -->

```scss
@mixin userrem($size) {
  $shebei:320px ,375px ,414px, 360px, 384px,435px,$size;
  html{
    @each $i in $shebei{
      @media screen and (min-width:$i){
        font-size: 100px*($i/$size);
      }
    }
  }
}
@include userrem(640px);  //设计图的尺寸大小
```
