---
title: vue3面试题
date: 2023-03-12
tags:
categories: 面试题
---

### created 和 mounted 的区别

created 在实例创建后调用，可以获取到 this，无法获取到 dom, 可以进行一些接口请求
mounted 在实例挂载 dom 后调用，可以通过 nextTick 获取 dom 元素，ref 等

### computed 和 watch 有什么区别

1、computed 是计算属性，watch 是监听 data 中数据变化
2、computed 有缓存功能， 可以设置 get 和 set, 可以直接在模板中用
3、watch 支持异步， computed 不支持
4、computed 第一次加载就触发，watch 第一次加载不触发
5、computed 中的函数必须调用 return；watch 不是。
6、使用场景：
computed：一个属性受到多个属性影响，如：购物车商品结算。
watch：一个数据影响多条数据，如：搜索数据。

immediate 组件创建时刻执行与否
immediate: true,第一次加载时监听（默认为 false）
deep 深度监听 不推荐使用(非常的消耗性能)
监听的属性是对象的话 不开启 deep 对象子属性变化不会触发 watch
开启了 deep 对象内部所有子属性变化 都会触发 watch

### 权限控制

在 beforeEach 中判断是否已经请求过路由表，有就用本地的，没有就重新请求，然后通过 addRoutes 添加到 vue 路由中去，这样会保证每次刷新路由都不会丢失

### vue2 和 vue3 的区别

- 1、 vue3 引入 composition Api 相比 vue2 代码块逻辑更清楚。
- 2、 vue3 使用函数，import 导入，可以提高摇树优化（Tree Shaking）并减少代码体积
- 3、 vue3 对 ts 支持更友好
- 4、 vue3 可以像 react 那样把重复功能封装成一个 hooks 和组件解耦， 而 vue2 是通过 mixins，多个 mixins 容易造成命名冲突
- 5、 vue3 采用 pinia 进行状态管理，pinia 有更好的 ts 支持，并且抛弃 mutations 简化了状态管理
- 6、 vue3 采用 proxy 进行数据劫持，可以劫持整个对象，vue2 采用 object.defineProperty 需要遍历数据，并且无法监听对象新增删除熟悉和对数组操作
<!-- more -->

### reactive 重新赋值整个对象会发生什么？

reactive 重新赋值会导致丢失响应式，可以通过挨个赋值，或者 object.assign()来保证响应式不丢失

### ref 和 reactive 的区别

ref 定义：基本数据类型
reactive 定义：对象（或数组）数据类型
ref 内部通过 object.defineProperty 来实现响应式，ref 也可以用来定义对象和数组，它内部通过 reactive 的转为代理对象
reactive 通过 proxy 来实现响应式
ref 是 reactive 的语法糖
