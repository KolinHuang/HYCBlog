---
title: Spring：Spring中的JdbcTemplate
author: Kol Huang
date: 2020-09-05 21:43:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



## 概述

是Spring框架中提供的一个对象，是对原始Jdbc API对象的简单封装。Spring框架为我们提供了很多的操作模版类。

```markdown
* 操作关系型数据的：
		JdbcTemplate
		HibernateTemplate
* 操作nosql数据库的：
		RedisTemplate
* 操作消息队列的：
		JmsTemplate
```



## JdbcTemplate实现CRUD操作

Account的封装策略Spring以帮我们写好`BeanPropertyRowMapper<Account>(Account.class)`，无需自己实现

```java
package com.yucaihuang.jdbcTemplate;

import com.yucaihuang.domain.Account;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Component;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

/**
 * JdbcTemplate的CRUD操作
 */

@Component
public class JdbcTemplateDemo2 {

    public static void main(String[] args) {
        //获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");

        JdbcTemplate jdbcTemplate = (JdbcTemplate) ac.getBean("jdbcTemplate");
        //保存
//        jdbcTemplate.update("insert into account(name,money)values(?,?)","eee",333);
        //更新
//        jdbcTemplate.update("update account set name=?,money=? where id=?","update",2222,6);
        //删除
//        jdbcTemplate.update("delete from account where id=?",5);
        //查询所有
//        List<Account> accounts = jdbcTemplate.query("select * from account where id > ?", new AccountRawMapper(), 6);
//        List<Account> accounts = jdbcTemplate.query("select * from account where id > ?", new BeanPropertyRowMapper<Account>(Account.class), 6);
//        for (Account account : accounts) {
//            System.out.println(account.toString());
//        }

        //查询返回一行一列（使用聚合函数，但不加group by子句）
        Integer cnt = jdbcTemplate.queryForObject("select count(*) from account where id > ?", Integer.class, 5);
        System.out.println(cnt);
    }

}
/**
 * 定义Account的封装策略
 */
class AccountRawMapper implements RowMapper<Account>{

    /**
     * 把结果集中的数据封装到Account中，然后由Spring把每个Account加到集合中
     * @param resultSet
     * @param i
     * @return
     * @throws SQLException
     */
    public Account mapRow(ResultSet resultSet, int i) throws SQLException {
        Account account = new Account();
        account.setId(resultSet.getInt("id"));
        account.setName(resultSet.getString("name"));
        account.setMoney(resultSet.getFloat("money"));
        return account;
    }
}

```





## JdbcTemplate在Spring的IoC中的使用

```java
package com.yucaihuang.dao.impl;

import com.yucaihuang.dao.IAccountDao;
import com.yucaihuang.domain.Account;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.support.JdbcDaoSupport;

import java.util.List;

public class AccountDaoImpl extends JdbcDaoSupport implements IAccountDao {


    public List<Account> findAll() {
        List<Account> accounts = getJdbcTemplate().query(
                "select * from account",
                new BeanPropertyRowMapper<Account>(Account.class));
        return accounts;
    }

    public List<Account> findAccountById(Integer id) {
        return getJdbcTemplate().query("select * from account where id = ?",
                new BeanPropertyRowMapper<Account>(Account.class),
                id);
    }

    public void updateAccount(Account account) {
        getJdbcTemplate().update("update account set name=?,money=? where id = ?",account.getName(),account.getMoney(),account.getId());
    }
}

```

配置Bean.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">


    <bean class="com.yucaihuang.dao.impl.AccountDaoImpl" id="accountDao">
        <property name="jdbcTemplate" ref="jdbcTemplate"/>
    </bean>


    <bean class="com.yucaihuang.service.impl.AccountServiceImpl" id="accountService">
        <property name="accountDao" ref="accountDao"/>
    </bean>



    <bean id="driverManagerDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="qazwsxedc7410"/>
    </bean>

    <bean class="org.springframework.jdbc.core.JdbcTemplate" id="jdbcTemplate">
        <property name="dataSource" ref="driverManagerDataSource"/>
    </bean>

</beans>
```

