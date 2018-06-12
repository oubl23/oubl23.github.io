---
title: Spring Aop
date: 2018-04-18 17:32:54
categories: spring
tags: 
    - spring
    - aop    
---

## 基本名词
### advice（通知）
何时执行
1. before
2. After
3. After-returning
4. After-throwing
5. around

### join poin(连接点)
通知的执行点
这个点可以是调用方法时、抛出异常时、甚至修改一个字段时

### poincut 切点
决定哪些地方执行通知

### aspect 切面
切面就是通知和切点的结合，何时何处执行什么功能

### introduction 引入
引入允许我们向现有的类添加新方法或属性

### weaving
将切面和程序和并在一起
