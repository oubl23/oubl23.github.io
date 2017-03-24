---
title: ffcs报表开发
date: 2017-03-23 17:32:54
categories: ffcs
tags:
---
# 访问url
1. 查询部分：http://localhost:7001/workshop/query/show_result.html?result=8889&sid=LHSP-20160914-00002
2. 完整部分: http://localhost:7001/workshop/queryTemplate/TemplateDemo.html?panel=8889&grid=8889
# 查询数据部分
## sql_cfg
sql_text 里面存了查询语句用查询数据库的数据

## Sql_Param_Cfg 
配置查询使用得sql参数，绑定查询参数

## get_value_cfg_field
配置显示的字段

## get_value_cfg 
配置页面显示的 grid参数,绑定sql_cfg

## get_value_show_cfg
配置表格显示的样式


# 自定义面板部分
## BASE_PANEL
创建面板

## PANEL_TOOLBAR
创建工具栏

## PANEL_TOOLBAR_CFG
BASE_PANEL  和 PANEL_TOOLBAR绑定

## PARAM_COMPONENT
配置查询控件中，可以使用sql语句填充参数

## PANEL_TOOLBAR_BY_PARAM
PARAM_COMPONENT 和 PANEL_TOOLBAR 绑定

## PANEL_TOOLBAR_BY_MENU;
未用到

