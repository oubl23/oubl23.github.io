---
title: pl/sql控制语句
date: 2017-03-20 11:22:56
tags:
  - pl/sql
  - oracle
---

# 条件分支
1. if then;
2. if then else;
3. if then elseif else;

**实例**
```sql
create or repalce  procedure sp_pro6(spName varchar2)is
v_sal emp.sal%type;
begin
select sal into val from emp where ename=spName;
if v_sal<2000 then
update emp set sal=sal*1.1 where ename=spName;
end if;
end;
```
# 循环
## loop -- end loop;
```sql
loop
  insert into user1 values(v_num, spName);
  exit when v_num=10;
  v_num:=v_num+1;
end loop;

```

##  while loop -- end loop;

```sql
whiel v_num<20 loop

insert into user1 valuse(v_num,spNmae);
v_num:=v_num+1;
end loop;
end;
```

## for
```sql
begin
  for i in reverse 1..10 loop
  insert into users valus(i,'dabao');
  end loop;
end;
```

# 顺序控制语句
## goto
```sql
begin
loop
 dbms_output.put_line('输出i='||i);
 if i=10 then
 got end_loop;
 end if;
 i:=i+1;
 end loop;
<<end_loop>>
dbms_output.output.put_line('循环结束');
end;
```

## null
不会执行任何操作，并且会将控制语句传递到下一条语句
```sql
declare
v_num test.name%type:=1;
begin
  while v_num<20 loop
    v_num:=v_num+1;
    if v_num <> 10 then
      insert into test values(v_num);
    else
      null;
    end if;
    dbms_output.put_line(v_num);
  end loop;
end;

```
