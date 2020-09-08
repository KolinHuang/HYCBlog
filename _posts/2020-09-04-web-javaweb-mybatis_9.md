---
title: Mybatis：延迟加载和立即加载
author: Kol Huang
date: 2020-09-04 19:25:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



# 延迟加载和立即加载

问题：在一对多中，当我们有一个用户，它有100个账户。

​			在查询用户的时候，要不要把关联的账户查出来？

​			<span style="color:blue">在查询用户时，用户下的账户信息应该是在需要被使用时，查询出来。</span>

​			在查询账户的时候，要不要把关联的用户查出来？

​			<span style="color:blue">在查询账户时，账户的所属用户信息应该一起被查询出来。</span>





## 延迟加载（一对多，多对多）

在真正使用数据时，才发起查询，不用的时候不查询。按需加载（懒加载）

在`IUserDao.xml`文件的`resultMap`属性中配置`collection`：

```xml
<collection property="accounts" ofType="account" column="id" select="com.hyc.dao.IAccountDao.findAccountByUid"></collection>
```

在`IAccountDao.xml`文件中配置：

```xml
    <select id="findAccountByUid" resultType="account">
        select * from account a where uid = #{uid}
    </select>
```

即，让`IUserDao.xml`调用`IAccountDao`中的`findAccountByUid`，由`IAccountDao`查询account信息。

在`sqlMapConfig,xml`中配置：

```xml
<!--    配置参数-->
    <settings>
<!--        开启mybatis支持延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="true"/>
    </settings>
```

开启延迟加载功能



## 立即加载（多对一，一对一）

只要一调用方法，马上发起查询。