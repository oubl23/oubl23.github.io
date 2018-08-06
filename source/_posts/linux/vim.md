---
title: vim 
date: 2018-08-06 10:00:00
categories: linux
tags:
---
vim 文本编辑器

# vim 乱码
## 配置文件
set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
set enc=utf8
set fencs=utf8,gbk,gb2312,gb18030
## 以某种编码打开文件
:e ++enc=someencoding somefile