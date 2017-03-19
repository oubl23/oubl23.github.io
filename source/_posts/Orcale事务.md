---
title: Orcale事务
date: 2017-03-16 22:15:12
categories: 学习笔记
tags: ['orcale','sql']
---

# orcale session
每一个登录的用户都是独立的session他们不相互通信

# 事务
## 缓冲区
数据库DML操作实际上存在缓冲区中commit后才提交到数据库中

## ROLLBACK
更新操作回到原点

## commit
真正更新的操作，无法ROLLBACK

## 注意
莫个session更新数据表的时候，其他sesson无法更新，必须之前的session提交后
