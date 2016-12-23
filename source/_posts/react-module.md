---
title: React-组件
date: 2016-12-1
tags:
categories: React
---
-------------------------------------------------
一个组件，有自己的结构，有自己的逻辑，有自己的样式，会依赖一些资源，会依赖某些其他组件。比如日常写一个组件，比较常规的方式：

* 通过前端模板引擎定义结构
* JS文件中写自己的逻辑
* CSS中写组件的样式
* 通过RequireJS、SeaJS这样的库来解决模块之间的相互依赖

那么在React中是什么样子呢？

<!-- more -->
## 结构和逻辑
在React的世界里，结构和逻辑交由`JSX`文件组织，React将模板内嵌到逻辑内部，实现了一个JS代码和HTML混合的JSX。

### 结构

在JSX文件中，可以直接通过`React.createClass`来定义组件：
```bash
var CustomComponent = React.creatClass({
  render: function(){
    return (<div className="custom-component"></div>);
  }
});
```
通过这种方式可以很方便的定义一个组件，组件的结构定义在`render`函数中，但这并不是简单的模板引擎，我们可以通过js方便、直观的操控组件结构，比如我想给组件增加几个节点：
```bash
var CustomComponent = React.creatClass({
  render: function(){
    var $nodes = ['h','e','l','l','o'].map(function(str){
      return (<span>{str}</span>);
    });
    return (<div className="custom-component">{$nodes}</div>);
  }
});
```
通过这种方式，React使得组件拥有灵活的结构。那么React又是如何处理逻辑的呢？

### 逻辑

写过前端组件的人都知道，组件通常首先需要相应自身DOM事件，做一些处理。必要时候还需要暴露一些外部接口，那么React组件要怎么做到这两点呢？

#### 事件响应

比如我有个按钮组件，点击之后需要做一些处理逻辑，那么React组件大致上长这样：
```bash
var ButtonComponent = React.createClass({
  render: function(){
    return (<button>屠龙宝刀，点击就送</button>);
  }
});
```
点击按钮应当触发相应地逻辑，一种比较直观的方式就是给button绑定一个onclick事件，里面就是需要执行的逻辑了：
```bash
function getDragonKillingSword() {
  //送宝刀
}
var ButtonComponent = React.createClass({
  render: function(){
    return (<button onclick="getDragonKillingSword()">屠龙宝刀，点击就送</button>);
  }
});
```
但事实上`getDragonKillingSword()`的逻辑属于组件内部行为，显然应当包装在组件内部，于是在React中就可以这么写：
```bash
var ButtonComponent = React.createClass({
  getDragonKillingSword: function(){
    //送宝刀
  },
  render: function(){
    return (<button onClick={this.getDragonKillingSword}>屠龙宝刀，点击就送</button>);
  }
});
```
这样就实现内部事件的响应了，那如果需要暴露接口怎么办呢？

#### 暴露接口

事实上现在`getDragonKillingSword`已经是一个接口了，如果有一个父组件，想要调用这个接口怎么办呢？

父组件大概长这样：
```bash
var ImDaddyComponent = React.createClass({
  render: function(){
    return (
    <div>
    //其他组件
    <ButtonComponent />
    //其他组件
    </div>
    );
  }
});
```
那么如果想手动调用组件的方法，首先在`ButtonComponent`上设置一个`ref=""`属性来标记一下，比如这里把子组件设置成`<ButtonComponent ref="getSwordButton"/>`，那么在父组件的逻辑里，就可以在父组件自己的方法中通过这种方式来调用接口方法：
```bash
this.refs.getSwordButton.getDragonKillingSword();
```
看起来屌屌哒~那么问题又来了，父组件希望自己能够按钮点击时调用的方法，那该怎么办呢？

#### 配置参数

父组件可以直接将需要执行的函数传递给子组件：
```bash
<ButtonComponent clickCallback={this.getSwordButtonClickCallback}/>
```
然后在子组件中调用父组件方法：
```bash
var ButtonComponent = React.createClass({
  render: function(){
    return (<button onClick={this.props.clickCallback}>屠龙宝刀，点击就送</button>);
  }
});
```
子组件通过`this.props`能够获取在父组件创建子组件时传入的任何参数，因此`this.props`也常被当做配置参数来使用

屠龙宝刀每个人只能领取一把，按钮点击一下就应该灰掉，应当在子组件中增加一个是否点击过的状态，这又应当处理呢？

#### 组件状态

在React中，每个组件都有自己的状态，可以在自身的方法中通过`this.state`取到，而初始状态则通过`getInitialState()`方法来定义，比如这个屠龙宝刀按钮组件，它的初始状态应该是没有点击过，所以getInitialState方法里面应当定义初始状态clicked: false。而在点击执行的方法中，应当修改这个状态值为click: true：
```bash
var ButtonComponent = React.createClass({
  getInitialState: function(){
    //确定初始状态
    return {
      clicked: false
    };
  },
  getDragonKillingSword: function(){
    //送宝刀

    //修改点击状态
    this.setState({
      clicked: true
    });
  },
  render: function(){
    return (<button onClick={this.getDragonKillingSword}>屠龙宝刀，点击就送</button>);
  }
});
```
这样点击状态的维护就完成了，那么render函数中也应当根据状态来维护节点的样式，比如这里将按钮设置为disabled，那么render函数就要添加相应的判断逻辑：

```bash
render: function(){
  var clicked = this.state.clicked;
  if(clicked){
    return (<button disabled="disabled" onClick={this.getDragonKillingSword}>屠龙宝刀，点击就送</button>);
  }else{
  return (<button onClick={this.getDragonKillingSword}>屠龙宝刀，点击就送</button>);
  }        
}
```
