---
title: Spring：声明式事务控制
author: Kol Huang
date: 2020-09-05 21:44:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---





## 基于XML

1. 配置事务管理器

2. 配置事务的通知

   此时需要导入事务的约束：tx和aop名称空间和约束

   使用tx:advice标签配置事务通知

   ​	属性：

   ​		id：给事务通知起一个唯一标识

   ​		transaction-manager：给事务通知提供一个事务管理器引用

3. 配置AOP中的通用切入点表达式

4. 建立事务通知和切入点表达式的对应关系

5. 配置事务的属性

   在事务的通知tx:advice标签内部配置

   ​	isolation：指定事务的隔离级别，默认值为default，表示使用数据库的默认隔离级别

   ​	propagation：用于指定事务的传播行为。默认值时REQUIRED，表示一定会有事务，增删改的选择。查询方法可以选择SUPPORTS

   ​	read-only：用于指定事务是否只读。只有查询方法才能设置为true。默认值为false，表示读。

   ​	timeout：用于指定事务的超时时间。默认值是-1，表示永不超时。如果指定了数值，以秒为单位。

   ​	rollback-for：用于指定一个异常，当产生该异常时，事务回滚，产生其他异常时，事务不回滚。没有默认值。表示任何异常都会回滚。

   ​	no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚，产生其他异常时，事务回滚。没有默认值。表示任何异常都会回滚。



```xml
<!--    Spring中基于XML的声明式事务控制配置步骤-->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="driverManagerDataSource"/>
    </bean>

    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED" read-only="false"/>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
        </tx:attributes>
    </tx:advice>

    <aop:config>
        <aop:pointcut id="pt1" expression="execution(* com.yucaihuang.service.impl.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"/>
    </aop:config>
```



## 基于注解

1. 配置事务管理器
2. 开启Spring对注解事务的支持
3. 在需要事务支持的地方使用@Transactional注解

## 基于纯注解