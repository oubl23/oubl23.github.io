---
title: spirng基本概念
date: 2017-12-16 17:32:54
categories: java
tags: 
    - spring
    - java
---
## ioc
控制反转：将创建bean维护bean之间的关系的控制权从程序中转移到spring容器中（applicationContext.xml)中

## di
依赖注入：和ioc概念一致

## aop
面向切面编程：提取出交叉功能(叫切面)，日后织如该功能
1. 定义接口
2. 编写对象（被代理的对象）
3. 编写通知（前置通知或者后置通知）
4. bean.xml配置
5. 配置被代理对象
6. 配置通知
7. 配置代理对象，是ProxyFactoryBean的实例
