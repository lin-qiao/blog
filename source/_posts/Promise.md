---
title: 手写一个promise
date: 2020-01-07
tags:
categories: javascript进阶
---


```javascript
function myPromise(fn) {
    _this = this
    this.state = 'pending' //promise状态
    this.success = undefined //成功信息
    this.error = undefined  //失败信息
    this.onFulfilledFunc = [] //保存成功回调
    this.onRejectedFunc = []  //保存失败回调
    fn(resolve, reject)   //立即执行参数函数
    function resolve(value){
        //两个==="pending"，保证了状态的改变是不可逆的
        if(_this.state === 'pending'){
            // 依次执行成功回调函数
            _this.onFulfilledFunc.forEach(fn => fn(value))
            _this.state = 'resolved'
            _this.success = value
        }
    }
    function reject(value){
        //两个==="pending"，保证了状态的改变是不可逆的
        if(_this.state === 'pending'){
            //依次执行失败回调函数
            _this.onRejectedFunc.forEach(fn => fn(value))
            _this.state = 'rejected'
            _this.error = value
        }
    }
}
//同时，需要在 myPromise的原型上定义链式调用的 then方法：
myPromise.prototype.then = function(onFulfilled, onRejected){
    ///等待态，此时异步代码还没有走完
    if(this.state === 'pending'){
        if(typeof onFulfilled === 'function'){
            this.onFulfilledFunc.push(onFulfilled) //保存回调
        }
        if(typeof onRejected === 'function'){
            this.onRejectedFunc.push(onRejected) //保存回调
        }
    }
    //成功态，执行成功回调函数
    if(this.state === 'resolved'){
        if(typeof onFulfilled === 'function'){
            onFulfilled(this.success)
        }
    }
    //失败态， 执行失败回调函数
    if(this.state === 'rejected'){
        if(typeof onFulfilled === 'function'){
            onRejected(this.error)
        }
    }
}


new myPromise(function(resolve, reject){
    resolve(1)
}).then(res => {
    console.log(res)
})

```
