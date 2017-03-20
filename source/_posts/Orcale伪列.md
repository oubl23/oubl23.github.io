---
title: Orcale伪列
date: 2017-03-16 22:40:30
categories: 学习笔记
tags:
  - orcale
---

# ROWNUM
不固定动态生产，可以用于分页
```sql
select * from (
  select *, ROWNUM rn from where ROWNUM<10) temp
  where temp.rn > 5
```
# ROWIID
不会重复，先插入的ROWID最小，可以用于删除重复的字段
## 组成
1. 数据对象号
2. 相对文件号
3. 数据块号
4. 数据行号
