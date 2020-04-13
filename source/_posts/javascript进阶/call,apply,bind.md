---

title: 手写call,apply,bind

date: 2020-02-27

tags:

categories: javascript进阶

---

#### 手写call

```javascript
Function.prototype.myCall = function(context){
    context = context || window
     //利用对象调用函数，谁调用this指向谁的机制
    context.fn = this  
    // 截取除context外的其他参数
    const args = [...arguments].slice(1)
    const result = context.fn(...args)
    delete context.fn
    return result
}
```

#### 手写apply

```javascript
Function.prototype.myApply = function (context) {
    context = context || window
    //利用对象调用函数，谁调用this指向谁的机制
    context.fn = this
    //apply第二个参数为数组
    const result = arguments[1]? context.fn(...arguments[1]) : context.fn()
    delete context.fn
    return result
}
```

#### 手写bind

bind 跟call,apply 有所不同

-	bind 返回的是一个函数
-	bind 类似于函数柯里化的过程，像这样

```javascript
// 柯里化
function test(x) {
    return function(y){
        return x + y;
    }
};
test(1)(2); // 3
// bind
function test2(a, b) {
    return a + b;
}
test2.bind(null, 1)(2); // 3
```

-	bind返回的函数如果当作构造函数会怎样？

```javascript
function test(a,b){
    this.a = a
    this.b = b
}
test.prototype.sum = function () {
    return this.a + this.b
}

var t1 = new test(1,2)
t1.sum()   //3, this指向t1

var test2 = test.bind(null, 3)
var t2 = new test2(4)
t2.sum()   //7, this指向t2
```

按正常来看，test.bind(null,3)肯定指向window, 所以将绑定函数当作构造函数的时候，this指向无效，但是还可以预设参数

```javascript
Function.prototype.myBind = function (context) {
    const _this = this
    // 截取除context外预传递的其他参数
    const argsP = [...arguments].slice(1)
    return function fn(...args) {
        // 因为返回了一个函数，可以 new fn()，所以需要判断
        if(this instanceof fn){
            //new的时候不需要改变this,所以直接new 原始函数
           return  new _this(...argsP, ...args)
        }else{
            //改变this,合并参数
           return  _this.apply(context, argsP.concat([...args]))
        }
    }
}
```
