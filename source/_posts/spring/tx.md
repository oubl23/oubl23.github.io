---
title: 事务管理
date: 2018-04-25 17:32:54
categories: spring
tags: 
    - spring
---

## 事务管理
启用jdbcTemplate事物管理，见代码
```
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"
        p:dataSource-ref="dataSource"
    />
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"
        p:dataSource-ref="dataSource"
    />
    <tx:annotation-driven transaction-manager="transactionManager"/>
```
先要声明一个事物管理器，然后在spring中启用这个驱动/事物管理器
javaConfig 可以使用@EnableTransactionManagement声明

