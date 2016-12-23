---
title: React ES6语法
date: 2016-12-1
tags:
categories: React
---
-------------------------------------------------
ES6, 即ECMAScript6, JavaScript的新标准, 书写更加规范, 代码更加优雅. React Native推荐使用ES6的类写法代替传统的模块, 即使用extends React.Component代替React.createClass.

<!-- more -->

## 模块定义
在ES6中, RN模块使用class形式, 代替var形式, 使代码更加规范.旧的写法, 方法或变量使用逗号(“,”)间隔.
```bash
var Feed = React.createClass({});
```
ES6的写法, 方法或变量结束使用分号(“;”).
```bash
class Feed extends Component {}
```
## 引入导出
### 引入
引入使用`import Story from './Story.js';`替换`var Story = require('./Story');`

### 导出
导出使用`export default Feed;`替换`module.exports = Feed;`

## 初始化状态
在ES6中, 使用class类的形式, 因此初始化在构造器(`constructor`)中进进行. 旧的写法, 使用`getInitialState`, 使用return返回状态.
```bash
var Feed = React.createClass({
  getInitialState() {
    return {
      dataSource: new ListView.DataSource({
        rowHasChanged: (row1, row2) => row1 !== row2
      }),
      loaded: false,
      isAnimating: true,
      isRefreshing: false
    };
  }
});
```
ES6的写法, 使用`constructor`构造器, 直接设置state状态.
```bash
class Feed extends Component {
  constructor(props) {
    super(props);
    this.state = {
      dataSource: new ListView.DataSource({
        rowHasChanged: (row1, row2) => row1 !== row2
      }),
      loaded: false,
      isAnimating: true,
      isRefreshing: false
    }
  }
}
```
## this绑定
在ES6中, 注意this的作用域, 由于使用类的写法, 所以this仅仅指代当前的类, 对于内部类需要重新指定this, 指定位置可以任选. 旧的方式直接写, 使用传递的props属性.
```bash
var Feed = React.createClass({
  // 直接使用this.props属性
  renderStories(story) {
    return (
      <Story story={story} navigator={this.props.navigator}></Story>
    );
  }

  render() {
    return (
      <ListView
        testID={"Feed Screen"}
        dataSource={this.state.dataSource}
        renderRow={this.renderStories}
        .../>
    )
  }
});
```
ES6的写法. 在内部类中, 需要重新绑定this. 三种实现方式: 构造时, 调用时, 使用时.
```bash
class Feed extends Component {
  // 构造器直接绑定方法.
  constructor(props) {
    super(props);
    this.renderStories = this.renderStories.bind(this);
  }
}
```
```bash
class Feed extends Component {
  // 调用时, 绑定this.
  render() {
    return (
      <ListView
        testID={"Feed Screen"}
        dataSource={this.state.dataSource}
        renderRow={(story) => this.renderStories(story)}
        .../>
    )
  }
}
```
```bash
class Feed extends Component {
  // 使用时, 绑定this
  render() {
    return (
      <ListView
        testID={"Feed Screen"}
        dataSource={this.state.dataSource}
        renderRow={this.renderStories.bind(this)}
        .../>
    )
  }
}
```
> 注意绑定this失败, 会发生未定义错误, 即undefined is not an object.

### 箭头函数
箭头函数实际上是在这里定义了一个临时的函数，箭头函数的箭头=>之前是一个空括号、单个的参数名、或用括号括起的多个参数名，而箭头之后可以是一个表达式（作为函数的返回值），或者是用花括号括起的函数体（需要自行通过return来返回值，否则返回的是undefined）。

需要注意的是，不论是bind还是箭头函数，每次被执行都返回的是一个新的函数引用，因此如果你还需要函数的引用去做一些别的事情（譬如卸载监听器），那么你必须自己保存这个引用
```bash
// 正确的做法
class PauseMenu extends React.Component{
    constructor(props){
        super(props);
        this._onAppPaused = this.onAppPaused.bind(this);
    }
    componentWillMount(){
        AppStateIOS.addEventListener('change', this._onAppPaused);
    }
    componentDidUnmount(){
        AppStateIOS.removeEventListener('change', this._onAppPaused);
    }
    onAppPaused(event){
    }
}
```
