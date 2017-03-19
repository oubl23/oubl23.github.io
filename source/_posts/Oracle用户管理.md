---
title: Oracle用户管理
date: 2017-03-17 14:20:52
categories: 学习笔记
tags:
  - orcale
---

# 创建用户
>只有管理员权限的用户才能添加用户

**语法**
```sql
CREATE USER 用户名 IDENTIFIED BY 密码
```

# 权限授予

 **语法**

 ```sql
 GRANT 权限 TO 用户
 GRANT CREATE SESSION TO TEST; //创建session权限
 GRANT CONNECT, RESOURCE TO TEST;
 ```

## 一次性赋予多权限

### CONNECT 连接权限

### RESOURCE 资源权限

## 表格的操作权限
**语法**

```sql
GRANT CREATE, DELETE ON scott.emp TO TEST;
```

# 回收权限
**语法**

```sql
REVOKE 权限 ON 表 from 用户
REVOKE SELECT,DELETE ON SCOTT.EMP FROM TEST;
```

# 修改密码

**语法**

```sql
ALTER USER 用户名 IDENTIFIED BY 密码;
```

## 第一次登录修改密码
**语法**

```sql
ALTER USER 用户名 password expire;
```

# 锁住用户

**语法**

```sql
ALTER USER 用户名 ACCOUNT LOCK ;
```

## 解锁用户

**语法**

```sql
ALTER USER 用户名 ACCOUNT UNLOCK ;
```
