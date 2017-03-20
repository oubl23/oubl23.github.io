---
title: Orcale语句和函数
date: 2017-03-16 10:50:39
categories: 学习笔记
tags:
  - orcale
---

# 语句分类
## DML-数据操作语句
用于检索和修改数据

## DDL-数据定义语句
用于定义数据结构，创建、修改或者删除数据对象

## DCL-数据控制语句
用于定义数据库用户权限

# 单行函数
## 字符串函数
### TO_CHAR
```sql
TO_CHAR(SYSDATE,'fmyyyy-mm-dd hh24:mi:ss') FROM dual;
TO_CHAR(8989898989898,'L999,999,999,999,999') FROM dual;
//return ￥8,989,898,989,898 L当前语言函数中的货币
```

### TO_DATE
```sql
TO_DATE('1992-11-13','yyyy:mm:dd');
```

## 通用函数
### DVL()
```sql
//将null变为指定符号
NVL(comm,0)
```
### DECODE()
相当于switch语句
```sql
//DECODE(数值/列,判断值1,显示值1,....) 如果没配置的地方用空显示
```

## 统计函数
1. count 一定会放回具体的数字空就是0
2. agv 空的会是null?
3. sum 空的会是null?
