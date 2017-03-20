---
title: Orcale序列
date: 2017-03-17 11:21:36
categories: 学习笔记
tags:
  - orcale
  - sql
---

# 序列
>用于自动增长的列

**基本语法**
```sql
CREATE SEQUENCE SEQUENCE1
[INCREMENT BY N][START WITH N]
[MAXVALUE N| NOMAXVALUE]
[MINVALUE N| NOMINVALUE]
[CYCLE|NOCYCLE]
[CACHE N|NOCACHE]
```

**创建简单序列**
```sql
CREATE SEQUENCE myseq;
```

## 序列的操作
- **下一个值** myseq.nexval
- **当前值** myseql.currval

# 同义词
> 只适用于orcale

```sql
CREATE SYNONYM EMP FOR SCOTT.EMP;
DROP SYNONYM EMP;
```
