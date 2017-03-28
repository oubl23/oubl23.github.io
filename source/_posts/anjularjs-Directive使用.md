---
title: anjularjs Directive使用
date: 2017-03-24 19:48:35
tags:
---

# DIrective使用
自定义组件

```javascipt
var app = angular.module('app',[]);

app.directive('hello',function(){
    return {
        restrict: 'E',
        template:'<div>Hello angularjs</div>'
    }
})

//html使用
<div ng-app="app">
    <hello></hello>
</div>
```
## restict
1. E element
2. A 属性
3. C class

## replace
替换掉自定义dom元素名称  

# link

```javascript
var app = angular.module('app',[]);

app.controller('AppCtrl',function($scope){
    $scope.loadMoreData=function(){
        alert("正在加载数据...")
    }
})

app.directive('enter',function(){
    return function(scope.element,attrs){
        element.bind('mouseenter',function(){
            scope.$apply(attrs.enter);//类似用onclick调用方式
        })
    }
})

// html
<div ng-controller="AppCtrl">
    <h1 enter="loadMoreData()">移动过来</h1>
</div>
```

## require 互相调用
require实现互相调用 **例子:**

```javascript
    app.directive('food',function(){
        return {
            restrict:'E',
            controller:function($scope){
                $scope.foods = [];
                this.addApple= function(){
                    $scope.food.push('apple');
                }
                this.addOrange= function(){
                    $scope.food.push('orange');
                }
                this.addBanana= function(){
                    $scope.food.push('banana');
                }
            },
            link:function(scope,element,attrs){
                element.bind('mouseenter',function(){
                    console.log(scope.foods);
                });
            }
        }
    })

    app.directive('apple',function(){
        return{
            require:'food',
            link:function(scope,element,attrs,foodCtrl){
                foodCtrl.addApple();
            }
        }
    })

    app.directive('orange',function(){
        return{
            require:'food',
            link:function(scope,element,attrs,foodCtrl){
                foodCtrl.addOrange();
            }
        }
    })

    app.directive('banana',function(){
        return{
            require:'food',
            link:function(scope,element,attrs,foodCtrl){
                foodCtrl.addBanana();
            }
        }
    })
```

# 监听

```javacript
temple:<div><input ng-model="txt"></div></div>
link:function(scope.element){
    scope.$warch('txt',function(newVal){
        if(newVal === 'error'){
            element.addClass('alert-box alert')
        }
    })
}