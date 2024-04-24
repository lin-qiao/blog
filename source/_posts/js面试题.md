---
title: js 面试题
date: 2023-03-12
tags:
categories: 面试题
---

### 工厂模式和单例模式

工厂模式是一种创建对象的模式，它将对象的创建过程封装在一个函数中，通过这个函数可以创建出具有相同特征的对象单例模式则是一种确保一个类只有一个实例

#### 工厂模式在 Vue 中的其他例子：

- 组件生成器：
  当需要动态创建组件时，可以使用工厂函数来根据参数生成不同的组件实例。例如，一个弹窗组件可能需要根据传入的不同配置（如标题、内容等）来动态生成不同的弹窗实例。

- 工具类函数：
  工厂模式也可以用于创建一些工具类函数，这些函数封装了特定的逻辑，并返回一个新的函数或对象。比如，创建一个用于处理 API 请求的工厂函数，根据不同的 API 路径和参数返回不同的请求处理函数。

#### 单例模式在 Vue 中的其他例子：

- 全局事件管理器：
  在 Vue 应用中，可能需要一个全局的事件管理器来监听和触发事件。这个事件管理器可以作为单例存在，确保整个应用中只有一个实例，方便在不同组件之间进行事件通信。

- 全局状态管理器（非 Vuex）：
  虽然 Vuex 是 Vue 中常用的状态管理库，但有时候我们可能希望实现一个简单的全局状态管理器作为单例存在。这个管理器可以管理一些简单的全局状态，并在需要时提供访问和修改这些状态的接口。

- API 服务层：
当与后端 API 进行交互时，可以创建一个单例的 API 服务层来封装所有的 API 请求。这样，整个应用就可以通过这个唯一的 API 服务实例来发送请求，避免了在每个组件中都单独创建 API 请求逻辑的冗余。
<!-- more -->

### 浏览器有哪些单位

px em rem vh vw
px 是绝对单位
vh vw 根据浏览器窗口大小改变
em rem 是相对单位 em 根据自身或者祖先元素的 font-size 来计算 rem 根据 html 标签的 font-size 来计算

### rem 是什么

rem 是移动端适配的一个单位
赋值给 html 标签给一个 font-size = `设备可视区域尺寸 / 设计图尺寸 * 100`
然后 1rem 就等于这个 fontsize 大小
mete viewport 可以用来设置手机屏幕可视区域大小
width=device-width 设置视区域宽度为设备宽度

### 304 是什么，如何清除 304

当浏览器访问静态资源的时候，如果已经访问过这个文件，会优先访问本地缓存的文件，这时状态就会提示 304
清除 304 可以通过给文件路径添加一个可修改的参数，比如 v=1 或者时间戳

### 设置 flex 后哪些属性会失效

flex 给了子元素空间分布和对齐能力
浮动 float 清除浮动 clear:both，垂直居中 vertical-align
如果子元素是行内元素，会变成块元素，display:inline; 父元素 text-align 等会失效

### v-for key 的作用

key 的作用主要用来优化 diff 算法， sameVnode 会根据 key 和标签名判断是否是同一节点, 设置了 key 可以减少 diff 的比较
key 不要设置成 index， 数组增加删除会导致 index 变化，diff 算法比较变多

### new 操作符执行了哪些操作

1、创建一个对象
2、把该对象的`__proto__`指向 fn 的 prototype
3、fn 的 this 指向该对象

### 如何保证构造函数只能被 new 调用

1、通过 instanceof 判断
2、通过 new.target 判断
3、使用 class

### 正则

匹配符：
d? d 出现 0/1 次
a＊ a 可以出现 0/多次
a+ a 出现一次以上
a｛6｝ a 出现 6 次
a｛2，｝ a 出现 2 次以上
a｛2，6｝ a 出现 2-6 次
匹配多个字符：
(ab)+ ab 出现一次以上
或运算：
a (cat|dog) 匹配 a cat or a dog
a cat|dog 匹配 a cat or dog
字符类：
匹配由 abc 构成的数据【abc】+ abc 出现一次以上 abc aabbcc
【a-zA-Z0-9】 ABCabc123
^ 排除 【^0-9】 匹配 0-9 之外的数据(包括换行符)
元字符
\d 数字字符 \d+ 匹配一个以上的数字
\D 非数字字符
\w 单词字符 单词 数字 下划线即英文字符
\W 非单词字符
\s 空白符 包含空格和换行符
\S 非空白字符
\b 单词的边界 单词的开头或结尾 单词与符号之前的边界
\B 非单词的边界 符号与符号 单词与单词的边界
. 任意字符不包含换行符
\. 表示. 通过\进行了转意
^ 匹配行首 $ 匹配行尾
＊+｛｝贪婪匹配
<strong><b>https://www.wondershare. com</strong></b>
<.+> 会匹配整串 因为是贪婪匹配
<.+?> 只匹配两个标签代码，➕? 设置为懒惰匹配

### 前端首屏加载优化

1、减少入口文件体积，常用的方法是路由懒加载
2、静态资源 cdn 缓存，vue, vue-router axios 等库通过 cdn 引入
3、UI 框架按需引入，不要全部引入
4、避免组件重复打包，通过 splitChunks 代码分割
5、图片资源压缩
6、gzip 压缩
7、图片懒加载
8、资源预渲染

### js 判断对象或者数组的方法

instanceof 根据原型链来判断 修改原型链或者 iframe 上的原型会有问题
Object.prototype.toString.call() [Symbol.toStringTag] 可以修改他返回的值
constructor.name
isArray 判断数组
JSON.stringify()
.length 判断数组
Object.keys().length 判断对象

### ajax 流程

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "https://example.com/data", true);
xhr.send("key1=value1&key2=value2");
xhr.onreadystatechange;
xhr.onload;
```

### let const var 区别

var 存在全局污染，会添加到 window 上
let const 有块级作用域
let const 存在暂时性死区
var 可重复声明， let const 不可以

### 进程线程

进程是程序运行开辟的一块内存空间
运行代码的人叫线程
