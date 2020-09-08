---
title: Mybatis：连接池以及事务控制
author: Kol Huang
date: 2020-09-04 19:21:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---

## 连接池概念

连接池就是用于存储连接的一个容器，容器其实就是一个集合对象，该集合必须是线程安全的，不能两个线程拿到统一连接，集合必须实现队列的特性：先进先出。

在实际开发中都会使用连接池，可以减少我们获取连接所消耗的时间。



## mybatis中的连接池

mybatis连接池提供了3种方式的配置：

```markdown
配置的位置：主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式。
type属性的取值：
1.	POOLED 		采用传统的javax.sql.DataSource规范中的连接池	mybatis中有针对此规范的实现
2.	UNPOOLED	采用传统的获取连接的方式，虽然也实现了javax.sql.DataSource接口，但是并没有使用池的思想。
3.	JNDI			采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到的DataSource是不一样的。
							注意：如果不是web或者maven的war工程，是不能使用的。tomcat服务器，采用的连接池是dbcp连接池。
```



## 事务原理和自动提交设置

```markdown
什么是事务
事务的四大特性ACID
不考虑隔离性产生的3个问题
解决办法：四种隔离级别
```

### mybatis中的事务

是通过`sqlSession`对象的`commit`方法和`rollback`方法实现事务的提交和回滚



### 自动提交事务设置

```java
//将openSession的值设为true，后面就无需手动提交事务
sqlSession = factory.openSession(true);
```

