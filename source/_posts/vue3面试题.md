---
title: vue3面试题
date: 2023-03-12
tags:
categories: 面试题
---

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
