---
title: 数据库
date: 2018-04-19 17:32:54
categories: spring
tags: 
    - java
    - spring
    - sql
---

## 数据源
### JDNI
```
<jee:jndi-lookup id="dataSource"
jndi-name="/jdbc/SpitterDS"
resource-ref="true" />
```
### 数据源连接池
- Apache Commons DBCP
- c3p0
- BoneCp
```
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
p:driverClassName="org.h2.Driver"
p:url="jdbc:h2:tcp://localhost/~/spitter"
p:username="sa"
p:password=""
p:initialSize="5"
p:maxActive="10" />
```
### JDBC驱动的数据源
- DriverManagerDataSource
- SimpleDriverDataSource
- SingleConnectionDataSource
推荐上面具有连接池的数据源
与具备池功能的数据源相比,唯一的区别在于这些数据源bean都没有提供连接池功能,所以没有可配置的池相关的属．
SingleConnectionDataSource有且只有一个数据库连接,所以不适
合用于多线程的应用程序,最好只在测试的时候使用。而DriverManagerDataSource和SimpleDriverDataSource尽管支持多线程,但是在每次请求连接的时候都会创建新连接,这是以性能为代价的
```
<bean id="dataSource"
class="org.springframework.jdbc.datasource.DriverManagerDataSource"
p:driverClassName="org.h2.Driver"
p:url="jdbc:h2:tcp://localhost/~/spitter"
p:username="sa"
p:password="" />
```
### 嵌入式数据源
嵌入式数据库作为应用的一部分运行,而不是应用连接的独立数据库服务器。尽管在生产环境的设置中,它并没有太大的用处,但是对于开发和测试来讲,嵌入式数据库都是很好的可选方案。这是因为每次重启应用或运行测试的时候,都能够重新填充测试数据
```
<?xml version="1.0" encoding="UTF-
8"?> <beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:jdbc="http://www.springframework.org/schema/jdbc"
xmlns:c="http://www.springframework.org/schema/c"
xsi:schemaLocation="http://www.springframework.org/schema/jdbc
http://www.springframework.org/schema/jdbc/spring-jdbc-3.1.xsd
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
...
<jdbc:embedded-
database id="dataSource" type="H2">
<jdbc:script location="com/habum
a/spitter/db/jdbc/schema.sql"/>
<jdbc:script location="com/habuma/sp
itter/db/jdbc/test-data.sql"/>
</jdbc:embedded-database>
...
</beans>
```

### 通过配置选择数据源
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:jdbc="http://www.springframework.org/schema/jdbc"
       xmlns:jee="http://www.springframework.org/schema/jee"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/jdbc
http://www.springframework.org/schema/jdbc/spring-jdbc-3.1.xsd
http://www.springframework.org/schema/jee
http://www.springframework.org/schema/jee/spring-jee-3.1.xsd
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
    <beans profile="development">
        <jdbc:embedded-database id="dataSource" type="H2">
            <jdbc:script location="com/habuma/spitter/db/jdbc/schema.sql"/>
            <jdbc:script location="com/habuma/spitter/db/jdbc/test-data.sql"/>
        </jdbc:embedded-database>
    </beans>
    <beans profile="qa">
        <bean id="dataSource"
              class="org.apache.commons.dbcp.BasicDataSource"
              p:driverClassName="org.h2.Driver"
              p:url="jdbc:h2:tcp://localhost/~/spitter"
              p:username="sa"
              p:password=""
              p:initialSize="5"
              p:maxActive="10"/>
    </beans>
    <beans profile="production">
        <jee:jndi-lookup id="dataSource"
                         jndi-name="/jdbc/SpitterDS"
                         resource-ref="true"/>
    </beans>
</beans>
```

## Spring data
Spring Data 可以方便的构建简单的select,update,insert,delete, 只需要声明接口而不需要实现只要这些接口是继承自JpaRepository
如：readSpitterByFirstnameOrLastnameOrderByLastname()// 其中表示数据库的Spitter可以隐藏如果参数中有他的Map对象的话
```
//xml配置
<jpa:repositories base-package="com.habuma.spittr.db" />

//javaConfigCode
@EnableJpaRepositories(basePackages="com.habuma.spittr.db")
```
