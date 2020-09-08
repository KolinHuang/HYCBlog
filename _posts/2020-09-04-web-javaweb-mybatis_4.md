---
title: Mybatis：Properties/TypeAliases/Package
author: Kol Huang
date: 2020-09-04 19:20:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---

## Properties

配置`properties`可以在标签内部配置连接数据库的信息，也可以通过属性引用外部配置文件信息。在资源路径下新建外部配置文件`jdbcConfig.properties`:

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/eesy_mybatis
jdbc.username=root
jdbc.password=qazwsxedc7410
```

在`SqlMapConfig.xml`文件中配置`properties`标签：

```xml
<configuration>

    <properties resource="jdbcConfig.properties">
      
    </properties>
  	<!--    配置环境-->
    <environments default="mysql">
<!--        配置mysql的环境-->
        <environment id="mysql">
<!--            配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
<!--            配置数据源（连接池）-->
            <dataSource type="POOLED">
<!--                配置连接数据库的4个基本信息-->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
  
```

`resource`属性用于指定配置文件的位置，是按照类路径的写法来写，并且必须存在于类路径下。

`url`属性要求按照`Url`的写法来写地址：`协议 主机 端口 URI`--->`file:///URI`



## typeAliases/package

使用`typeAliases`配置别名：

```xml
<!--    使用typeAliases配置别名，只能配置domain中类的别名-->
    <typeAliases>
<!--        typeAlias用于配置别名，type属性指定的是实体类全限定类名，alias属性指定别名，当指定了别名就不再区分大小写-->
        <!--        <typeAlias type="com.hyc.domain.User" alias="user"></typeAlias>-->
<!--        用于指定要配置的别名的包，当指定之后，该包下的实体类都会注册别名，并且类名就是别名，不再区分大小写-->
        <package name="com.hyc.domain"/>
    </typeAliases>
```

```xml
    <mappers>
<!--        <mapper resource="com/hyc/dao/IUserDao.xml"/>-->
<!--        <mapper class="com.hyc.dao.IUserDao"/>-->
<!--        package标签用于指定dao接口所在的包，当指定了之后就不再需要写mapper、resource或者class了-->
        <package name="com.hyc.dao"/>
    </mappers>
```



