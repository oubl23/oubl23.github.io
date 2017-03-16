---
title: Orcale查询
date: 2017-03-16 14:46:12
tags:
---

# 基本查询语法
```sql
select [DISTINCT] * | 分组字段1 [别名] [,分组字段2 [别名],...] | 统计字段
from 表名称 [别名],[表名词 [别名],...]
[where 条件...]//分组前的过滤,不能使用统计函数
[group by 分组字段1, [分组字段2], ...]
[having 分组后的额过滤函数，可以使用分组统计 函数] //分组后的再次过滤
[order by 排序字段 ASC|DESC [排序字段 ASC|DESC ...]];

## 使用分组后的限定条件
1. 分组函数可以单独用，后面不能出现其他字段
2. 字段分组后，只能出现分组的字段和统计函数，不能出现其他字段
3. 分组允许嵌套，但是分组之后不能出现任何其他字段

```
# 简单查询
## 消除列重复-DISTINCT
```sql
SELECT  DISTINCT job FROM emp;
```

## 语句拼接
```sql
SELECT 'A'||EMPNO||'B' FROM EMP;

```
# 多表查询
性能考虑，尽量回避多变查询
## 笛卡儿积
### 产生效果
两表查询会产生两表相乘的数据，多于我们需要的数据  
```sql
select  * from dept,emp;
```

### 消除笛卡儿积
```sql
select * from dept,emp where emp.deptno = dept.deptno;
//值只是显示上消除，查询中还存在，性能很差，有笛卡儿积问题
```

## 他表关联
```sql
select * from dept,emp where emp.deptno = dept.deptno;
```
## 自表关联
```sql
select * from emp e ,emp m where e.mgr = m.empno;
```

## 两个以上关联条件
```sql
select e.empno,m.name ,d.loc
from emp e ,emp m , dept d
where e.mgr = m.empno and e.deptno = d.deptno;
```

## between and 关联
```sql
select e.empno,e.sal,m.name ,d.loc
from emp e ,emp m , salgrade s
where e.mgr = m.empno and e.sal between s.losal and s.hisal;
```

## 左右连接
即查询的参考点，比如我要查询出A的所有数据与之关联的B为空也无所谓

1. (+)= :这是右边连接
2. =(+) :这是左连接
**注意这是orcale独有的其他数据库如mysql参考sql1999语法**

## sql1999 连接
### 交叉连接(CROSS JOIN)
用于产生笛卡儿积
```sql
SELECT * FROM emp CROSS JOIN DEPT;
```

### 自然连接(NATURAL JOIN)
自动查找关联字段
```sql
SELECT * FROM emp NATURAL JOIN DEPT;
```

### JOIN .... USING
用户指定关联字段
```sql
SELECT * FROM emp  JOIN DEPT USING(deptno);
```
### JOIN .... ON
用户指定关联条件
```sql
SELECT * FROM emp JOIN DEPT USING(emp.deptno = dept.deptno);
```

### 连接方向改变
1. 左（外）连接：LEFT OUTER JOIN .... ON;
2. 右（外）连接：RIGHT OUTER JOIN ... ON;
3. 全（外）连接：FULL OUTER JOIN .... ON;
```sql
select * from emp right outer join dept on(emp.deptno = dept.deptno);
```

## 多表查询补充
### 查询表
1. 做法一：
```sql
select * from emp;
```
2. 做法二：
```sql
select count(*) from emp;
//然后决定如何查询，推荐
```

# 子查询
## WHERE
子查询一般只返回单行单列、多行单列、单行多列的数据；
### 单行单列
```sql
select * from emp where sal > (select avg(sal) from emp);
```

## 多行单列
```sql
select * from emp where (job, sal)
 = (select job,sal from emp where ename = "ALLEN")
```

## 单行多列
1. in(=any)
2. (>any 大于其中最小) (<any 小于其中最大)
3. (>all 大于其中最大) (<all 小于其中最小)

## FROM
子查询返回的一般是多行多列的数据，当作一张临时表出现
```sql
SELECT d.deptno,d.dname,d.loc,temp.count,temp.avg
from dept t (
  select deptno dno ,count(empno) count ,avg(sal) avg
  from emp
  groupby deptno)temp
  where d.deptno(+) = temp.dno
```
