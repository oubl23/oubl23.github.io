---
title: Orcale表得创建和管理
date: 2017-03-17 08:44:51
tags: ['orcale','sql']
---

# DDL操作
## 创建对象
> CREATE 对象名称 ....

```sql
CREATE TABLE 表名称(
  字段 类型 [DEFAULT],
  ......
)
```
### 表的复制
```sql
CREATE TABLE EMP20 AS SELECT * FROM emp where deptno = 20;

CREATE TABLE EMP20 AS SELECT * FROM emp where 1 = 20;
//只复制表结构 ，以上只支持ORCALE
```
### 修改表名称
```sql
RENAME table1 TO table2
//ORCALE独有
```

### 截断表
- delete 可以回滚
2. 彻底清楚表的全部资源，TRUNCATE table tablename;

## 删除对象
> DROP 对象名称 ....

### 表的删除
```sql
DROP TABLE person [PURGE];
```
### 闪回技术(FLASHBACK)
- SHOW RECYCLEBIN
2. 恢复语法 FASHBACK TABLE 表名称 TO BEFORE DROP;
3. 删除回收站 PURGE TABLE 表名称
4. 清空回收站 PURGE RECYLEBIN


## 修改对象
> ALTER 对象名称 ....

### 字段添加
```sql
ALTER TABLE member ADD(age NUMBER [default ]);
```
> 如果没有设置default 新加字段都为空，如果有default如果有值就为设置vi

### 字段修改
```sql
ALTER TABLE member MODIFY (name VARCHARAR2(100) [default ]);
```
> 一般不用

## 一般创建过程
### 1. 删除表
```sql
DROP TABLE nation PURGE;
```
### 2. 创建表
```sql
CREATE TABLE nation(
  name VARCHAR2(50)
)
```
### 3. 添加测试数据
```sql
INSERT INTO nation(name) VALUES('中国');
INSERT INTO nation(name) VALUES('美国');
INSERT INTO nation(name) VALUES('巴西');
INSERT INTO nation(name) VALUES('荷兰');
COMMIT;
```
### 4. 使用查询语句
```sql
SELECT N1.NAME, N2.name
FROM NATION N1, NATION N2
WHERE N1.NAME <> N2.NAME;
```
# 数据类型
- 字符串
**VARCHARAR2(n)**
- 整数
**NUMBER(n)**
- 小数
**NUMBER(n,m)**
- 日期
**DATE**
- 大文本
**LOB**
- 大对象
**BLOB**

# 约束
## 非空约束 **NOT NULL**
```sql
name varchar2(50) not null;
```
## 唯一约束 **UNIQUE**
```sql
email varchar2(50) unique;
//指定约束名
email varchar2(50),
CONSTRAINT UK_email unique(email)
```

## 主键约束 **primary key**
> 相当与 非空+唯一,开发而言只设置一个主键，语法上允许多主键
**复合主键** 只有有一个不一样就可以

```sql
mid number primary key;
//指定名字
mid number,
CONSTRAINT PK_email primary key(email)
```

## 检查约束 **check**
> 检查输入数据

```sql
//指定名字
sex varchar2(2),
CONSTRAINT CK_email check(sex in (0,1,2))
```
## 主外建约束 **FOREIGN KEY**
> 这个由两张表中进行，两张表存在关系

> 使用删除操作的时候推荐先删除子表，再删除父表

```sql
DROP TABLE MEMBER PURGE;
DROP TABLE BOOK PURGE;
CREATE TABLE MEMBER(
  MIN NUMBERM
  NAME VARCHAR2(50) NOT NULL,
  CONSTRAINT PK_MID PRIMARY KEY (MID)
);
CREATE TABLE BOOK(
  BID NUMBER,
  TITLE VARCHAR2(50) NOT NULL,
  MID NUMBER,
  CONSTRAINT PK_BID PRIMARY KEY (BID),
  CONSTRAINT FK_MID POREIGN KEY (MID) REFERENCES MEMBER(MID)
);

```
### 集联删除 **ON DELETE CASCADE**
删除父表会删除子表数据
```sql
 CONSTRAINT FK_MID POREIGN KEY (MID) REFERENCES MEMBER(MID) ON DELETE CASCADE
```

删除父子为空
```sql
CONSTRAINT FK_MID POREIGN KEY (MID) REFERENCES MEMBER(MID) ON DELETE SET NULL
```

## 修改约束
>尽量不要改变约束

###  **添加**
```sql
ALTER TABLE 表名 ADD CONSTRAINT 约束名 约束条件
```
>存在违反约束的数据则无法添加约束

###- **删除** ALTER TABLE 表名 DROP  CONSTRAINT 约束名 约束条件

## 查询约束
```sql
select * from user_constraints;
```
