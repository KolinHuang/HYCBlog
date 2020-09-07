---
title: Spring：概述
author: Kol Huang
date: 2020-09-05 21:38:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



Spring是分层的Java SE/EE应用`full-stack`轻量级开源框架，以`IoC`（`Inverse Of Control`: 反转控制）和`AOP`（`Aspect Oriented Programming`: 面向切面编程）为内核，提供了展现层Spring MVC和持久层Spring JDBC以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库。



## Spring的优势

```markdown
1. 方便接耦，简化开发
2. AOP 编程的支持
3. 声明式事务的支持
4. 方便程序的测试
5. 方便程序的测试
6. 方便集成各种优秀框架
7. 降低JavaEE API的使用难度
8. 源码是经典学习范例
```



## Spring的体系结构

![javaweb_Spring体系结构](/HYCBlog/assets/img/web/javaweb_Spring体系结构.jpg)

注：此为spring4.x版本，在最新的spring5.x中，web模块的portlet组件已经被废弃，同时增加了用于异步响应式处理的WebFlux组件。

- **Spring Core：** 基础,可以说 Spring 其他所有的功能都需要依赖于该类库。主要提供 IoC 依赖注入功能。
- **Spring Aspects** ： 该模块为与AspectJ的集成提供支持。
- **Spring AOP** ：提供了面向切面的编程实现。
- **Spring JDBC** : Java数据库连接。
- **Spring JMS** ：Java消息服务。
- **Spring ORM** : 用于支持Hibernate等ORM工具。
- **Spring Web** : 为创建Web应用程序提供支持。
- **Spring Test** : 提供了对 JUnit 和 TestNG 测试的支持。