---
title: angular概述
date: 2016-08-07 17:04:35
tags:
categories: angularjs
---

以前开发（web或者移动端）前端主要使用jQuery+原生js,如果使用某些前端UI框架的话，它自己还可能提供一些API可以使用。而且目前很多UI框架都是基于jQuery的，所以说一下由jQuery跨到angularjs跨度较大，研究了一段时间的angularjs ,下面从整体上说说感受吧:

<!-- more -->

### 关于和jquery的比较
首先angular是一个mvc框架，它与jquery不同之处在于，前者致力于mvc代码解耦，采用model,controller以及view方式去组织代码，而后者提供给你了很多APi函数，你可以不用写很多原生js去实现比较复杂的效果，比如说动画，$.animate,这样的效果如果需要原生js来写的话，代码量将会比较庞大；
其次，jQuery没有定义你的代码如何组织，你可以将它放在一个单独的js文件中进行引用，也可以直接写在页面中采用script标签进行包裹，甚至可以直接以内联的方式写在html标签中，但是angularjs会将一个HTML页面分成若干个模块，每个模块都可以自己的scope，service以及directive，各个模块之间也可以进行通信，但是整体上结构是比较清晰的，就是说其代码组织方式是模块化的。
最后，jQuery的思想是先设计好页面，然后在已有页面的基础上进行dom操作后展示页面，但是angular的view可能仅仅是一个框架，对view的dom操作或者时间监听都是在directive中实现的，而且一般情况下很少自己直接去写Dom操作代码，只要你监听model。model发生变化后view也会发生变化。
### 关于适用场合
jQuery应该适用于大多数web开发，移动端也有（jQuerymobile），angularjs有人说更适合做SPA（我个人认为在手机上的SPA可能会引发性能上的问题，因为它的脏检查机制会影响性能），在web端，一些CRUD的应用或者管理类软件还是可以使用的（当然这里的理解可能不一定准确，会随着深入学习更多去了解和使用）。
### 关于UI的结合
开发任何产品都需要用到前端UI，目前很多UI是基于jQuery的，这意味着你如果要用angularjs和这些Ui组件的话，需要用angularjs的directive去重写些组件，这一过程是比较麻烦的，所幸的是，angular给我们提供了一些UI组件可以使用（web端主要是结合bootstrap前端组件），`http://angular-ui.github.io/` ，而在移动端主要是结合ionic框架 `http://ionicframework.com/`，但是随着angular的发展，很多HTML5的前端框架也慢慢集成了angularjs版本可供使用。
## 关于angularjs的特点
* 数据的双向绑定：这可能是其最激动人心的特性吧，view层的数据和model层的数据是双向绑定的，其中之一发生更改，另一方会随之变化，这不用你写任何代码！（想想jQuery方式下怎么做吧）
* 代码模块化，每个模块的代码独立拥有自己的作用域，model，controller等。
* 强大的directive可以将很多功能封装成HTML的tag,属性或者注释等，这大大美化了HTML的结构，增强了可阅读性；
* 依赖注入，将这种后端语言的设计模式赋予前端代码，这意味着前端的代码可以提高重用性和灵活性，未来的模式可能将大量操作放在客户端，服务端只提供数据来源和其他客户端无法完成的操作；


## 基本使用
```html
html ng-app="nw";
body ng-controller="mainCtrl";
```
```javascript
var nw=angular.module('nw',[]);
nw.controller('mainCtrl',[$scope,function ($scope) {

}])
```
## 模块
 ```html
 <script type="text/javascript" src="angular-animate.js"></script>
 //这就是一个模块
 ```
 ```javascript
 var nw=angular.module('nw',['ngAnimate'])
 ```

## 控制器
 ```javascript
 nw.controller('mainCtrl',['$scope',function ($scope) {
 }])
 ```
 > 双向数据绑定
 > 作用域(controller开始和结束标签之间)

## 装饰性指令

> 表达式(在装饰性指令中可以使用angular表达式)

```html
ng-repeat="v in list" $index $last $first $middel $odd $even
<!-- ng-repeat 在指令拥有最高优先级 -->
ng-bind==={{}};
ng-class="{a:(state===false)}";
ng-click

```
## 组件化开发
> angular 会以ajax请求在方式去调用我们写在html页面

> 在一个页面中只能用一个父元素把它们包起来

```javascript
test.directive('uqTitle',[function(){  // 传入驼峰变量 写到controller外面
    return {
      restrict:'E', //E(element) A(attribut) M C(class)
      template:'',
      templateUrl:'',//文件地址
      link:function ($scope,elem) { //对dom元素的操作
        //引入jquery后就可以使用$
      }
    }  
}])
```


## 在指令中使用jquery

angular内部提供了一个jqLite
angular.elements()===$();
在指令的link函数中不想使用jqLite
先引入jquery 再引入angular
angular会自动把jqLite 替换为jquery
在指令中去添加事件 操作DOM

## 使用动画
```html
<script type="text/javascript" src="angular-animate.js"></script>
```


>.ng-class  .ng-add   .ng-remove

>.ng-repeat .ng-enter .ng-enter-active

>           .ng-leave .ng-leave-active

>.ng-view   .ng-enter .ng-enter-active

>           .ng-leave .ng-leave-active

> .ng-if    .ng-enter .ng-enter-active

>           .ng-leave .ng-leave-active

## 使用路由
``` html
<script src="angular.route.js"></script>
<base href="/" target="_blank" />
<ng-view></ng-view>
```
``` javascript

var test = angular.module('test',['ngAnimate','ngRoute']);
test.controller('weixinCtrl',['$scope',function($scope){

}])
test.controller('lianxirenCtrl',['$scope',function($scope){

}])

test.config(['$routeProvider',function($routeProvider){
      $routeProvider.when('/',{
        controller:'weixinCtrl',
        templateUrl:'views/weixin.html'
      }).when('/weixin',{  
        controller:'weixinCtrl',
        templateUrl:'views/weixin.html'
      }).when('/lianxiren',{
        controller:'lianxirenCtrl',
        templateUrl:'views/lianxiren.html'
      }).otherwise({
          redirectTo:'/'
      });
}])
```
### //when('./weixin/:changshu') 下标可以传参   在controller函数中加入 $routeParams

```javascript
$routeProvider.when('/liaotian/:aaa'.{
  controller:'liaotianCtrl',
  templateUrl:'template/liaotian.html'
})
nw.controller('liaotianCtrl',['$scope','$routeParams',function ($scope,$routeParams) {
  var id=$routeParams.aaa;
  var list=list[id].liaotianjilv;
}])
```

## 使用内置服务
把服务依赖注入到控制器，指令，服务,过滤器

$scope $routeProvider $location


## touch

ng-swipe-left

## 服务

```javascript
test.factory('$student',[function(){
    return {
      getallStu:function () {
        return student
      }
    }  
  }])
test.controller('indexCtrl',['$scope','$student',function ($scope,$student) {

}])
```

## 自定义filter

```javascript
test.filter('test',[function () {
  return function(diyi){
     return '@'+diyi;
  }
}])

///注入controller
test.controller('indexCtrl',['$scope','$filter',function ($scope,$filter) {
  $scope.num=$filter('test')($scope.list);
}])
```

## 重要的服务

$scope, $location, $routeProvider
$routePramas
