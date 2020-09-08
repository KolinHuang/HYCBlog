---
title: Mybatis：mybatis中的缓存
author: Kol Huang
date: 2020-09-04 19:26:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



## Mybatis中的缓存



### 一级缓存

指的是mybatis中`SqlSession`对象的缓存。

当我们执行查询之后，查询的结果会同时存入SqlSession为我们提供的一块区域中。该区域的结构是一个Map。当我们再次查询同样的数据，mybatis会先去`sqlSession`中查询是否命中，命中的话直接拿出来用。

当`SqlSession`对象消失时，mybatis的一级缓存也就消失了。 



当调用`sqlSession`的修改，添加，删除，commit()，close()等方法时，就会清空一级缓存。



### 二级缓存

指的是Mybatis中`SqlSeesionFactory`对象的缓存。由同一个`SqlSessionFactory`对象创建的`SqlSession`共享其二级缓存。

二级缓存的使用步骤：

​	第一步：让mabatis框架支持二级缓存（在`SqlMapConfig.xml`中配置）

​	第二步：让当前的映射文件支持二级缓存（在`IUserDao.xml`中配置）

​	第三步：当当前的操作支持二级缓存（在`select`标签中配置）

