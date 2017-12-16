---
title: Oracle索引
date: 2017-03-19 11:45:03
categories: 学习笔记
tags:
  - oracle
---

# 原则
1. 为经常查询的字段创建索引，不经常查询的字段不用索引
2. 大表建立索引才有意义
3. 经常结合where的字段加索引
4. 不超过4个

# 缺点
1. 占用更大的空间(约1.2倍)
2. 更新数据的时候会使用更多的时间

# 创建索引
**语法**

```sql
CREATE INDEX 索引名 ON 表名(列名,...)
```

## 组合索引
经常合并查询的时候，可以创建组合索引，把能迅速减小范围的放后面

# 查看索引
>字段为unique的字段会自动添加索引
**语法**
```sql
select index_name, index_type from USER_INDEXS where table_name="表名"
```
