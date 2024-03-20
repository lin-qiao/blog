---
title: Vue 要点总结
date: 2017-03-03
tags:
categories: Vue
---

## 初识 vue

### (1) vue 到底是什么?

- 一个 mvvm 框架(库)、和 angular 类似
- 比较容易上手、小巧
- 官网:http://cn.vuejs.org/
- 手册： http://cn.vuejs.org/api/

<!-- more -->

### (2) vue 和 angular 区别?

- vue——简单、易学
  指令以 v-xxx
  一片 html 代码配合上 json，在 new 出来 vue 实例
  个人维护项目

适合: 移动端项目,小巧
vue 的发展势头很猛，github 上 start 数量已经超越 angular

- angular——上手难
  指令以 ng-xxx
  所有属性和方法都挂到$scope 身上
  angular 由 google 维护

合适: pc 端项目

- 共同点: 不兼容低版本 IE

### vue 基本雏形

- angular 展示一条基本数据:

```bash
var app=angular.module('app',[]);

app.controller('xxx',function($scope){ //C
$scope.msg='welcome'
})

#html:
<div ng-controller="xxx">{{msg}}</div>
```

- vue 展示一条基本数据:

```bash
# html:
<div id="box">{{msg}}</div>

var c=new Vue({
  el:'#box',  //选择器  class tagName
  data:{
    msg:'welcome vue'
  }
});
```

## 不失优雅

- Vue 虽然是一个比较轻量级的框架，简单轻量的同时还非常的人性化，数据写到 data 里边，对数据的操作写到 method 里边，写的代码更加规范易读，其提供的 API 也是非常的容易理解，同时也提供了一些很便捷的指令和属性。

```bash
（1）绑定click事件
<a v-on:click="doSomething"></a> => <a @click="doSomething"></a>
(2) 绑定动态属性
<a v-bind:href="url"></a> => <a :href="url"></a>
(3) 便捷的修饰符
<!-- 阻止单击事件冒泡 -->
<a @click.stop="doSomething"></a>
<!-- 只在按下回车键的时候触发事件 -->
<input @keyup.enter="submit">
(4) 实用的参数特性
<!-- debounce 设置一个最小的延时 -->
<input v-model="note" debounce="500">
```

## 小巧

说起小巧，那应该首先要关注下 Vue 的源码大小，Vue 的成产版本（即 min 版）源码仅为 72.9kb，官网称 gzip 压缩后只有 25.11kb，相比 Angular 的 144kb 缩小了一半。
小巧的一种好处就是可以让用户更自由的选择相应的解决方案，在配合其他库方面它给了用户更大的空间。
如 Vue 的核心默认是不包含路由和 Ajax 功能，但是如果项目中需要路由和 AJAX，可以直接使用 Vue 提供的官方库 Vue-router 及第三方插件 vue-resource，同时你也可以使用其他你想要使用的库或插件，如 jQuery 的 AJAX 等。

## 组件化

Vue 的组件化功能可谓是它的一大亮点，通过将页面上某一组件的 html、CSS、js 代码放入一个.vue 的文件中进行管理可以大大提高代码的维护性。

```bash
<template>
<div class="box" v-text="note"></div>
</template>

<script>
export default {
  data () {
    return {
      note: '这是一个组件的html模板！'
    }
  },
  method:{

  }
}
</script>

<style scoped>
.box {
  color: #000;
}
</style>
```

## vue-router

- 1. 布局

```bash
<router-link to="/home">主页</router-link> //router-link 会被加载成一个a标签
<router-view></router-view>
```

- 2. 路由具体写法

```bash
//组件
var Home={
  template:'<h3>我是主页</h3>'
};
var News={
  template:'<h3>我是新闻</h3>'
};

//配置路由
const routes=[
{path:'/home', componet:Home},
{path:'/news', componet:News},
];

//生成路由实例
const router=new VueRouter({
  routes
});

//最后挂到vue上
new Vue({
  router,
  el:'#box'
});
```

- 3. 重定向

之前 router.rediect 废弃了

```bash
{path:'*', redirect:'/home'}
```

- 4. 路由嵌套:

```bash
/user/username

const routes=[
{path:'/home', component:Home},
{
  path:'/user',
  component:User,
  children:[  //核心
  {path:'username', component:UserDetail}
  ]
},
{path:'*', redirect:'/home'}  //404
];
```

- 5. 动态传值

```bash
/user?id=4
:to="{path:'/user',query: {id:item.id}}"
```

- 6. 路由实例方法:

```bash
router.push({path:'home'});  //直接添加一个路由,表现切换路由，本质往历史记录里面添加一个
router.replace({path:'news'}) //替换路由，不会往历史记录里面添加
```

- 7. 路由切换可以与 animate.css 配合，产生页面切换动画的效果，类似 swiper

## vuex

### store

vuex 的核心是"store",简单的 store 如下：

```bash
// 如果在模块化构建系统中，请确保在开头调用了 Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

- 可以通过 store.state 来获取状态对象，以及通过 store.commit 方法触发状态变更：

```bash
store.state.count
store.commit('increment')
```

- Vuex 通过 store 选项，提供了一种机制将状态从根组件『注入』到每一个子组件中（需调用 Vue.use(Vuex)）

```bash
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
  <div class="app">
  <counter></counter>
  </div>
  `
})
```

- 当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState

```bash
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
```

- 当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。

```bash
computed: mapState([
// 映射 this.count 为 store.state.count
'count'
])
```

### Mutations

- 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutations 非常类似于事件,它会接受 state 作为第一个参数。

```bash
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

- 调用

```bash
store.commit('increment')
```

- 你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）：

```bash
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
store.commit('increment', 10)
```

- 在组件中提交 Mutations,你可以在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）。

```bash
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
    'increment' // 映射 this.increment() 为 this.$store.commit('increment')
    ]),
    ...mapMutations({
      add: 'increment' // 映射 this.add() 为 this.$store.commit('increment')
    })
  }
}
```

## 生命周期和钩子函数

![Image text](https://segmentfault.com/img/bVEs9x?w=847&h=572)

```bash
beforecreate : 举个栗子：可以在这加个loading事件
created ：在这结束loading，还做一些初始化，实现函数自执行
mounted ： 在这发起后端请求，拿回数据，配合路由钩子做一些事情
beforeDestory： 你确认删除XX吗？ destoryed ：当前组件已被删除，清空相关内容
```

## vue 常用的一些属性和方法

- data:用来存放数据
- methods:存放对数据和页面的操作
- computed:计算属性 对数据进行任何复杂逻辑
- mounted:在这发起后端请求，拿回数据，配合路由钩子做一些事情
- filters:过滤器
  vuex
- state:多个组件共享的数据，属性
- mutations 非常类似于事件,更改 Vuex 的 store 中的 state 的唯一方法
- getters 可以认为是 store 的计算属性,接受 state 作为其第一个参数
- actions 类似于 mutations 不同在于 actions 是提交 mutations，不是变更 state ，actions 可以包含异步操作
