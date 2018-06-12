---
title: javascipt设计
date: 2017-03-19 11:05:47
tags:
---
# 基础
## 封装
封装不仅仅是隐藏数据，还包括隐藏实现细节、设计细节以及隐藏对象的类型等

## this, call, apply
用 Function.prototype.call 或 Function.prototype.apply 可以动态地改变传入函数的 this 

## 闭包
简单来说就是通过返回一个函数的方式来保存对象中的变量
```javascript
##1
var extent = function() {
    var value = 0;
    return {
        call: function() {
                value++;
                console.log(value);
            }
    }
};
var extent = extent();
extent.call();
extent.call();
##2

var extent = {
        value: 0,
        call: function() {
            this.value++;
            console.log(this.value);
        }
    }
extent.call();
extent.call();
```

##　高阶函数
### 函数可以作为参数被传递
* 回调函数
```javascript
    var getUserInfo = function(userId, callback){
        callback(1);
    }

    getUserInfo(1, function(data){
        console.log(data);
    });

```
### 函数可以作为返回只输出

#　设计模式
## 单例模式
```javascript 
var getSingle = function(fn){
    var result;
    return function(){
        return result || (result = fn.apply(this.arguments));
    }
```

## 策略模式
定义一系列的算法,把它们一个个封装起来,并且使它们可以相互替换
1. 策略类
2. 环境类型context
```javacript
var calculateBonus = function( performanceLevel, salary ){
    if ( performanceLevel === 'S' ){
        return salary * 4;
    }
    if ( performanceLevel === 'A' ){
        return salary * 3;
    }
    if ( performanceLevel === 'B' ){
        return salary * 2;
    }
};
calculateBonus( 'B', 20000 );
calculateBonus( 'S', 6000 );
// 输出:40000
// 输出:24000
#策略
var strategies = {
    "S": function( salary ){
        return salary * 4;
    },
    "A": function( salary ){
        return salary * 3;
    },
    "B": function( salary ){
        return salary * 2;
    }
}
var calculateBonus = function( level, salary ){
    return strategies[ level ]( salary );
};
console.log( calculateBonus( 'S', 20000 ) );
console.log( calculateBonus( 'A', 10000 ) );
// 输出:80000
// 输出:30000
```

## 代理模式
1. 保护代理　用途：帮代理对象过略
2. 虚拟代理　用途：可以节省开销的时候用
3. 缓存代理　为一些开销大的运算结果提供暂时的存储,在下次运算时,如果传递进来的参数跟之前一致,则可以直接返回前面存储的运算结果

## 订阅发布模式
1. 在javascript中使用的很广泛
## 模板方法模式

1. 就是java中的继承

## 享元模式 flyweight
区分内部状态和外部状态
* 内部状态存储与对象内部
* 内部状态可以被一些对象分享
* 内部状态独立于具体的场景，通常不会改变。
* 外部状取决与具体的场景,并根据场景而变化，外部状态不能被共享
对象池: 重复利用资源,create资源的时候先在对象池里检查是否存在资源，不存在时候才申请新资源

## 职业链模式
环环紧扣，避免了多if/else的尴尬
不过我们需要避免过长的链条
### 异步
异步的返回值是直接运行到最后的返回值接受不到异步里面的return 可以采用函数调用的方式替代

## 中介者模式
所有的处理都是中介者进行，中介者就是中心

##  装饰者模式
当我们想为函数添加功能的时候。做法我们可以使用AOP方式来添加装饰者
```javascript
    var a = function(){
        console.log(1);
    }
    //change 不推荐
    var a = function(){
        console.log(1);
        console.log(2);
    }

    var _a = a;
    a = function(){
        _a();
        console.log(2);
    }
```
在html中绑定onload的用法

```javascript
    window.onlad = function(){
            console.log(1);
    }

    var _onload = window.onload || function(){};
    windows.onlad = function(){
        _onload();
        console.log(2);
    }
```
使用aop方式
```javascript
    Function.prototype.after = function(afterfn){
        var _self = this;
        return function(){
            var ret = _self.apply(this.arguments);
            afterfn.apply(this, arguments);
            return ret;
        }
    }
    window.onload = function(){
        console.log(1);
    }

    windows.onload = (window.onload||function(){}).after(function(){
        console.log(2);
    }).after(function(){});
```

## 状态模式
适合A->B->C->A这种情况

## 适配器模式
将函数转化，变得通用


# 设计原则和编程技巧
## 单一职责原则(SRP)
### 迭代器
```javascript
var appendDiv = function( data ){
    for ( var i = 0, l = data.length; i < l; i++ ){
        var div = document.createElement( 'div' );
        div.innerHTML = data[ i ];
        document.body.appendChild( div );
    }
};
appendDiv( [ 1, 2, 3, 4, 5, 6 ] );
//这里的appendDiv不仅仅渲染数据还迭代数据，万一入参变了就不通用了
//引入迭代器
var each = function( obj, callback ) {
    var value,
    i = 0,
    length = obj.length,
    isArray = isArraylike( obj );
    // isArraylike 函数未实现,可以翻阅 jQuery 源代码
    if ( isArray ) {
        // 迭代类数组
        for ( ; i < length; i++ ) {
            callback.call( obj[ i ], i, obj[ i ] );
        }
    } else {
        for ( i in obj ) {
            // 迭代 object 对象
            value = callback.call( obj[ i ], i, obj[ i ] );
        }
    }
    return obj;
};
var appendDiv = function( data ){
    each( data, function( i, n ){
        var div = document.createElement( 'div' );
        div.innerHTML = n;
        document.body.appendChild( div );
    });
};
appendDiv( [ 1, 2, 3, 4, 5, 6 ] );
appendDiv({a:1,b:2,c:3,d:4} );
```
- 代理模式
- 迭代器模式
- 单例模式
- 中介者模式

## 最少知识原则
单一职责原则指导我们把对象划分成较小的粒度,这可以提高对象的可复用性

##　开放－封闭原则
当需要改变一个程序的功能或者给这个程序增加新功能的时候,可以使用增加代码的方式,但是不允许改动程序的源代码
### 扩展函数
```javascript
Function.prototype.after = function( afterfn ){
    var __self = this;
    return function(){
        var ret = __self.apply( this, arguments );
        afterfn.apply( this, arguments );
        return ret;
    }
};
window.onload = ( window.onload || function(){} ).after(function(){
    console.log( document.getElementsByTagName( '*' ).length );
});
```

- 发布－订阅模式
- 模板方法模式
- 策略模式
- 代理模式
- 职责链模式
