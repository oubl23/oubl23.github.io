---
title: Orcale视图
date: 2017-03-17 11:09:00
tags: ['orcale','sql']
---

# 创建视图
>可以用于封装复杂查询

>视图的作用用于查询本来不应该用于更改

**语法**
```sql
CREATE OR REPLACE 视图名称 AS 子查询
[WITH CHECK OPTION][WITH READ ONLY]
```
