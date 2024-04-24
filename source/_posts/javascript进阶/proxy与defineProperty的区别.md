---
title: proxy与defineProperty的区别?
date: 2024-04-12
tags:
categories: javascript进阶
---

对象的基本操作呢？赋值？读取属性？删除属性？遍历？啊这些都是在语法层面上的操作。这其实是 js 这门语言为了让开发者使用起来更加方便或者是让程序的可读性更好，搞出来的一种独特的语法，那实际上，我们在使用语法的时候，在真正执行这些代码的时候，会转换为一个函数。

```javascript
obj.a; // 【GET】
obj.b = 3; //【SET】
delete obj.a; //【DELETE】
```

例如在读取对象属性的时候，运行的函数是 GET。这些内部运行的方法就是它的基本操作。

<!-- more -->

打开 ecma 262 文档看看对象的内部方法。
![](/blog/images/proxy与defineProperty的区别1.png)

`Object.defineProperty` 其实内部执行的就是 `DefineOwnProperty` 这个方法，你看接收的参数都一样。

来看看 `proxy`，它有一堆的内部方法，每一个方法都对应一个`捕获器`也可以叫做`陷阱`，这就对上了啊，用这个陷阱去拦截，然后你就掉进去了。所有的陷阱都是可选的，如果没有定义对应的陷阱，那就会保留源对象的默认行为。
![](/blog/images/proxy与defineProperty的区别2.png)

```javascript
const obj = {
  b: 40,
};
const handler = {
  get: function (target, prop) {
    console.log("prop", prop);
    return target[prop];
  },
};
const p = new Proxy(obj, handler);
console.log("p.b", p.b); // 40
```

这种情况，让`proxy`代理 obj，那`proxy`就拦截了 ojb 的内部方法`[[GET]]`，进而掉进了陷阱函数里了。
那`proxy`就是拦截所有的基本操作。`defineProperty` 啥也没拦截，而是在调用 `defineOwnProperty` 这个基本操作。
所以很多现象就是来源于这个这个情况，嗯就是上面这种。
比如说 vue2，调用数组的 `push` 方法，这一过程怎么拦截？比如设置 `length`，用 `Object.defineProperty` 进行处理，直接就报错了（length 属性无法被重新定义）。所以说，直接用通过数组对象去调原型里的 `push` 是没办法监听到的，是 vue 在中间插入一个对象，这个对象重写了数组的方法。这样实际上我们的数组对象去调的时候，调的其实是 vue 的那个对象，里面重写原型了。
而 vue3 就不用这么麻烦了，`proxy` 可以直接拦截到对象的基本操作，所以在陷阱函数里，就可以拦截到 `arr.push(1)`的操作了，包括是这个 `push`属性、`length` 属性、里面的变化的值等。

```javascript
const arr = [1, 2, 3];

const p = new Proxy(arr, {
  get(target, prop) {
    console.log("get", prop);
    return target[prop];
  },
  set(target, prop, value) {
    console.log("set", prop, value);
    target[prop] = value;
    return true;
  },
});

p.push(1);

// get push
// get length
// set 3 1
// set length 4
```
