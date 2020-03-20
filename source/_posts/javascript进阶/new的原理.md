---

title: new的原理，手写一个new

date: 2020-03-20

tags:

categories: javascript进阶

---

在调用 new 的过程中会发生以下四件事

-	1、生成一个新对象

-	2、将新对象的`__proto__`指向构造函数的`prototype`

-	3、将构造函数的`this`变为新对象并且执行构造函数中的代码（为新对象添加属性和方法）

-	4、返回新对象

手写一个new

```javascript
function _new(fn, ...arg) {
    var newObj = {}
    newObj.__proto__ = fn.prototype
    fn.call(newObj, ...arg)
    return newObj
}
```
