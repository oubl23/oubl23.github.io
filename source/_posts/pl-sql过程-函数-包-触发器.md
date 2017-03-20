---
title: pl/sql过程、函数、包、触发器
date: 2017-03-20 10:21:46
categories: 学习笔记
tags:
  - pl/sql
  - orcale
---

# 过程
用于执行指定的操作。输入输出参数可选，使用 **create procedure** 创建过程

```sql
create procedure sp_pro3(spName varchar2, new Sal number) is
begin
update emp set sal =newSal where enma=spName;
end;
/
```
# 函数
函数用于返回特点数据，头部必需要用return，一般返回一个值，使用 **create function** 创建函数
```sql
create function sp_fun2(spName varchar) return
number is yearSal number(7,2);
begin
select sal*12 + nvl(comm,0)* 12 into yearSal from emp where ename=spName;
return yearSal;
end;
/
```

## 调用函数
1. var income number;先定义变量
2. call sp_fun2('SCOTT') into:income;然后执行，赋值到变量中

# 包
用来组合过程和函数

```sql
create package sp_package us
procedure update_sal(name varchar2, newsal number);
function annual_income(name varchar2) return number;
end;

--包体
create or replace package body sp_package is
  procedure update_sal(name varchar2, newsal number)is
  begin
    update emp set sal=newsal where ename=name;
  end;

  function annual_income(name varchar2)
  return number is
  annual_salary number;
    begin
    select sal*12 +nval(comm,0) into annual_salary from emp
    where ename= name;
    return annual_salary;
  end;
end;

调用
exec sp_package.update_sal('scott',120);
```
# 触发器
是隐含执行的存储过程
