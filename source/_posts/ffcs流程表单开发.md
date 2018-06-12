---
title: ffcs流程表单开发
date: 2017-03-22 13:12:48
categories: ffcs
tags:
---

# url访问
http://127.0.0.1:7001/workshop/form/index.jsp?flowMod=16567


# 流程图要素
## 1. 流程图
管理服务-工作流管控-流程模板设计-专业管理流程
>记住流程ID和开始环节ID号


## 2. 流程表单
编写jsp文件

## 3. 流程表单表 - 存数据库
存入数据库并对应，示范名字 **CUT_SC_ITSM_BEATIFUAL_APPROVE**
>必须字段flow_ID number(12),REQUEST_ID number(20)


# 基本表介绍


```sql
-- 表单文件绑定--没有flow_id
select * from forms;  -- 保存流程表单的相关信息 保存页面路径 form_id
select * from data_tables; --建立的oracle表的信息 保存建立的表
select * from form_tables; --建立上两表的关系
select * from tache_form; --流程图与form_tables对应关系

-- 表单字段绑定 --没有flow_id
select * from form_element; --建立的oracle表的字段对应的表单元素的信息 对应ELEMENT_NAME 为html元素中的id值
select * from data_table_fields; --保存oracle表的字段信息，日期格式需要设置FIELD_FORMAT
select * from form_element_bind; --将上面两表建立关系
--以上内容到视频  00:37:51
```