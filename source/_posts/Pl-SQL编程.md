---
title: Pl/SQL编程
date: 2017-03-19 12:25:55
categories: 学习笔记
tags: ['orcale','pl/sql']
---
# pl/sql
## 优点
1. 模块化编程
2. 减少网络传输
3. 提高运行性能
4. 减少java编程的复杂度

## 特点
1. 过程，函数，触发器是pl/sql编写的,存放在ORCALE中
2. pl/sql是数据库过程语言
3. 过程、函数可以在java程序中调用
4. 安全性高，隐藏了表名等

## 缺点
1. 移植性不好，只是适用orcale

## 开发工具
1. sql_plus
2. pl/sql developer

## 范例
```sql
CREATE table mytest(
  name VARCHAR2(30),
  passwd VARCHAR2(30)
);

CREATE or REPLACE procedure  sp_prol is
begin
  insert into mytest values('大宝','m1234');
end;
```

### 查看错我信息
show error;

## 调用过程
**exec/call** 过程名(参数,...)

## 规范
### 注释
1. 单行 **--**
2. 多行 **/*...*/**

### 命名规范
1. 变量 v_sal
2. 常量 CREATE
3. 游标 emp_cursor
4. 例外 e_error

## 块
```sql
declear
/*定义部分*/
begin
/*执行*/
exception
/*例外部分*/
end;
```
