---
title: pl/sql变量
date: 2017-03-20 10:47:52
categories: 学习笔记
tags:
  - pl/sql
  - oracle
---

# 定义变量
```sql
--变长字符串
v_ename varchar2(10);
--定义小数
v_sal number(6,2);
--定义小数并赋值
v_sal2 number(6,2):=5.4;
--定义日期
v_hiredate date;
--定义boolean，不为空并初始false
v_vaild boolean not null default false;
```
## 使用表中的类型
```sql
v_ename emp.ename%type;
```

## 复合类型
### 记录
相当于c语言 **结构体**
```sql
type emp_record_type is record(
  name emp.ename%type,
  salary emp.sal%type,
  title emp.job%);
sp_record emp_record_type;

```

### 表
相当于 **数组**

```sql
--index by binary _integer 表示下标是整数
type sp_table_type is table of emp.ename%type index by binary_integer;

sp_table sp_table_type;
```

## 参照变量
### 游标变量 -ref cursor

```sql
declare
  --定义一个游标类型 sp_emp_cursor
  type  sp_emp_cursor is cursor;
  --定义变量
  test_cursor sp_emp_cursor;
  v_ename emp.ename%type;
  v_sal emp.sal%type;
begin
  --结合游标
  open test_cursor for select enaem,sal from emp where deptno=&no;
  loop
      fetch test_cursor into v_ename,v_sal;
      exit when test_cursor%notfound;
  end loop;
end;
```


### 对象类型变量
>用的比较少
