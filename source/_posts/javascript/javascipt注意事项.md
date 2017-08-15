---
title: javascipt注意事项
date: 2017-03-27 09:39:35
categories: 学习笔记
tags:
    - javascript
---

# 严格模式
**关键字**
```
'use strict';
```
在这个模式下需要遵循基本语法

# 自动添加分号

由于javascript会自动添加分号

```javascript
//不可取
return
{name:小明}

//要采用下面写法
return {
    name:小明
}
```

# this
JavaScript的函数内部如果调用了this，那么这个this到底指向谁？
如果单独调用函数，比如getAge()，就是指向window对象

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

var fn = xiaoming.age;//这样this指向fn
fn(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```
