---
title: Mybatis：多表关联查询
author: Kol Huang
date: 2020-09-04 19:23:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---





mybatis中的多表查询：

```markdown
表之间的关系有几种：
	一对多	用户和订单就是一对多
	多对一	订单和用户就是多对一
	一对一	一个人只能有一个身份ID
	多对多 一个学生可以对应多个老师，一个老师也可以对应多个学生
	特例：如果拿出每一个订单，他都只能属于一个用户，所以mybatis就把多对一看成了一对一。
```

## 示例：用户和账户

步骤：

```markdown
1. 建立两张表：用户表，账户表
		让用户表和账户表之间具备一对多的关系：需要使用外键再账户表中添加。
2. 建立两个实体类：用户实体类和账户实体类
		让用户和账户的实体类能体现出来一对多的关系
3. 建立两个配置文件
		用户的配置文件
		账户的配置文件
4. 实现配置
		当我们查询用户时，可以同时得到用户下包含的账户信息
		当我们查询账户时，可以同时得到账户的所属用户信息。
```



## 示例：用户和角色

一个用户可以有多个角色

一个角色可以赋予多个用户

步骤：

```markdown
1. 建立两张表：用户表，角色表
		让用户表和角色表具有多对多的关系。需要使用中间表，中间表中包含各自的主键，在中间表中是外键。
2. 建立两个实体类：用户实体类和角色实体类
		让用户和角色的实体类能体现出来多对多的关系。
		各自包含对方一个集合引用
3. 建立两个配置文件
		用户的配置文件
		角色的配置文件
4. 实现配置：
		当我们查询用户时，可以同时得到用户下所包含的角色信息。
		当我们查询角色时，可以同时得到角色下所包含的用户信息。
```
