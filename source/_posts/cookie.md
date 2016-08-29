---
title: JS中cookie的基本使用
date: 2016-08-25
tags:
categories: javascript
---
cookie是本身是HTML中document中的一个属性，可以用来保存一些简单的数据信息，比如用户名、密码等，提高一些网站的用户体验度。下面就来简单的说说cookie，它有下面几个特性：
* 1.有过期时间，这个可以设置，如果不设置默认是关闭浏览器则清除
* 2.有大小限制，一般cookie的条数不会超过50条，但因浏览器的不同也会有差异，单个cookie的大小不能超过2M
* 3.cookie是以键值对的形式保存在物理硬盘上的，类似json格式。

<!-- more -->

说了一些cookie的简要特性，下面就说其用法，直接上代码：

```javascript
//设置cookie  
//name是cookie中的名，value是对应的值，iTime是多久过期（单位为天）  
function setCookie(name,value,iTime){  
    var oDate = new Date();  
    //设置cookie过期时间  
    oDate.setDate(oDate.getDate()+iTime);  
    document.cookie = name+'='+value+';expires='+oDate.toGMTString();  
}  

//获取cookie  
function getCookie(name){  
    //cookie中的数据都是以分号加空格区分开  
    var arr = document.cookie.split("; ");  
    for(var i=0; i<arr.length; i++){  
        if(arr[i].split("=")[0] == name){  
            return arr[i].split("=")[1];  
        }  
    }  
    //未找到对应的cookie则返回空字符串  
    return '';  
}  

//删除cookie  
function removeCookie(name){  
    //调用setCookie方法，把时间设置为-1  
    setCookie(name,1,-1);  
}  
```

PS:在本地测试只有火狐才有效果，建议本地时用火狐测试.

## demo
利用cookie 实现保存tab下拉效果，刷新不会消失

### HTML
```HTML
<div class="test">
  分类1
</div>
<ul class="tMain">
  <li>11</li>
</ul>
<div class="test">
  分类2
</div>
<ul class="tMain">
  <li>21</li>
  <li>22</li>
</ul>
<div class="test">
  分类3
</div>
<ul class="tMain">
  <li>31</li>
  <li>32</li>
  <li>33</li>
</ul>
```
### css

```css
.tMain{
  display: none;
}
.tMain.active{
  display: block;
}
```

### js

```javascript
$('.test').each(function(i,v){
  //判断本地是否存有cookie
  if(getCookie('name'+i)!==''){
    var value=getCookie('name'+i); //取出name+i 的值
    if(value==='true'){
      $('.tMain').eq(i).addClass('active');  //如果值为true,给他加上active类
    }
  }else{
    var value=$('.tMain:eq('+i+')').hasClass('active'); //获取html是否有active 类
    setCookie('name'+i,value,7);  //存到cookie
  }
  $(this).click(function(){
    //ui
    $('.tMain').eq(i).toggleClass('active');
    //cookie
    var t=$('.tMain:eq('+i+')').hasClass('active');
    setCookie('name'+i,t);
  })
})
```
