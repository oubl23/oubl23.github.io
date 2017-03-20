---
title: pl/sql例外
date: 2017-03-20 16:58:03
tags:
---
# 例外处理

## no_data_found
```sql
v_ename emp.ename%type;
begin
select ename  into v_ename from emp where empno=&gno;
dbms_output.put_line('名字：'||v_ename);
exception
  when no_data_found then
  dbms_output.put_line('编号不存在!');
```

<!--more-->
## case_not_found

```sql
create or replace procedure sp_pro6(spno number)is
  v_sal emp.sal%type;
begin
  select sal into v_sal from emp where empno=spno;
  case
  when v_sal<1000 then
    update emp set sal=sal+100 where empno=spno;
  when v_sal<2000 then
    update emp set sal=sal+200 where empno=spno;
  end cas;
exception
  when case_not_found then
  dbms_output.put_line(v_sal);
```

## cursor_already_open

## dup_val_on_index
unquie字段适合会引发

## invaild_cursor
试图从打开的游标提取数据，或者关闭没有打开的游标

## invaild_number
输入数据有误

## too_many_rows
执行select into 语句的时候，返回多行

## zero_divide
当进行除数是0的除法操作的时候会引发

## value_error
变量长度超出范围

# 自定义例外

```sql
create or replace procedure ex_test(spNo number) is
myex exception;
begin
  update emp set sal=sal+1000 where empno=spNo;

  if sql%notfound then
  raise myex;
  end if;
exception
 when myex then
 dbms_output.put_line('没有更新任何用户');
end;


```
