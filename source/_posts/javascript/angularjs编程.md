---
title: angularjs
date: 2017-03-23 21:24:38
categories: 学习笔记
tags:
    - javascript
    - angularjs
---

# 常用标签
1. ng-app
2. ng-controller
3. ng-click
4. ng-if ng-hide
>ng-hide 会生成标签再hide,ng-if 如果不存在不会显示标签 

5. ng-checked ng-selected  
>根据boolean选择

6. ng-disabled 
7. ng-submi

# service
1. value
2. constant
3. factory
4. service
5. provider

# ng-repeat
## $index
下标
## $first,$last
boolean类型

# $http
## post
```javascript
$http.post(url)
.success(function(){

})
.error(){
    
}
```

## get
```javascript
$http.get(url)
.success(function(){

})
.error(){

}
```