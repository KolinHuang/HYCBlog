---
title: Mybatis：mybatis注解开发
author: Kol Huang
date: 2020-09-04 19:27:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---

## 环境搭建

1. 定义实体类`User.java`

2. 配置`SqlMapConfig.xml`文件，`jdbcConfig.properties`文件以及`log4j.properties`文件：

   ```properties
   jdbc.driver=com.mysql.jdbc.Driver
   jdbc.url=jdbc:mysql://localhost:3306/eesy_mybatis
   jdbc.username=root
   jdbc.password=qazwsxedc7410
   ```

   ```properties
   ### 设置###
   log4j.rootLogger = debug,CONSOLE,LOGFILE
   
   log4j.logger.org.apache.axis.enterprise=FATAL,CONSOLE
   
   ### 输出信息到控制台 ###
   log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
   log4j.appender.CONSOLE.layout = org.apache.log4j.PatternLayout
   log4j.appender.CONSOLE.layout.ConversionPattern = %d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
   
   #输出信息到文件
   log4j.appender.LOGFILE = org.apache.log4j.FileAppender
   log4j.appender.LOGFILE.File = /Users/huangyucai/IdeaProjects/exp_mybatis_log/axis.log
   log4j.appender.LOGFILE.Append = true
   log4j.appender.LOGFILE.layout = org.apache.log4j.PatternLayout
   log4j.appender.LOGFILE.layout.ConversionPattern = %d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
   
   
   ```

3. 在`IUerDao.java`接口中使用注解：

```java
public interface IUserDao {

    /**
     * 查询所有用户，同时获取到用户下所有账户的信息
     * @return
     * 在mybatis中，针对CRUD一共有四个注解：@Select, @Insert, @Update, @Delete
     */
    @Select("select * from user")
    List<User> findAll();

}
```

测试：

```java
package com.hyc.test;

import com.hyc.dao.IUserDao;
import com.hyc.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MybatisAnnoTest {
    /**
     * 测试基于注解的mybatis使用
     * @param args
     */
    public static void main(String[] args) throws IOException {
        //1.获取字节输入流
        InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.根据字节输入流构建SqlSessionFactory
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        //3.根据SqlSessionFactory生产一个SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //4.使用SqlSession对象获取Dao代理对象
        IUserDao userDao = sqlSession.getMapper(IUserDao.class);
        //5.执行Dao方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }
        //6释放资源
        sqlSession.close();
        is.close();
    }
}
```



## 单表CRUD操作（代理Dao方式）

CRUD操作：

```java
public interface IUserDao {

    /**
     * 查询所有用户，同时获取到用户下所有账户的信息
     * @return
     * 在mybatis中，针对CRUD一共有四个注解：@Select, @Insert, @Update, @Delete
     */
    @Select("select * from user")
    List<User> findAll();

    @Insert("insert into user(username,telephone,birthday,gender,address)" +
            "values(#{username},#{telephone},#{birthday},#{gender},#{address})")
    void saveUser(User user);

    @Update({"update user set username=#{username},telephone=#{telephone},birthday=#{birthday}," +
            "gender=#{gender},address=#{address} where id=#{id}"})
    void updateUser(User user);

    @Delete("Delete from user where id=#{id}")
    void deleteUser(Integer id);

}

```

测试：

```java
public class AnnotationCRUDTest {
    private SqlSessionFactory factory;
    private SqlSession session;
    private IUserDao userDao;
    private InputStream is;
    @Before
    public void init() throws IOException {
        is = Resources.getResourceAsStream("SqlMapConfig.xml");
        factory = new SqlSessionFactoryBuilder().build(is);
        session = factory.openSession();
        userDao = session.getMapper(IUserDao.class);
    }
    @After
    public void destory() throws IOException {
        session.commit();
        session.close();
        is.close();
    }
    @Test
    public void testSave(){
        User user = new User();
        user.setUsername("wa");
        user.setBirthday(new Date());
        user.setAddress("beijing");
        user.setGender("1");
        user.setTelephone("12221111");
        userDao.saveUser(user);
    }
    @Test
    public void testUpdate(){
        User user = new User();
        user.setId(13);
        user.setUsername("lin");
        user.setBirthday(new Date());
        user.setAddress("beijing");
        user.setGender("1");
        user.setTelephone("12111");
        userDao.updateUser(user);
    }
    @Test
    public void testDelete(){
        userDao.deleteUser(13);
    }
}

```

### 注解建立实体类与数据库列的对应关系

```java
public interface IUserDao {

    /**
     * 查询所有用户，同时获取到用户下所有账户的信息
     * @return
     * 在mybatis中，针对CRUD一共有四个注解：@Select, @Insert, @Update, @Delete
     */
    @Select("select * from user")
    @Results(id = "userMap",value = {
            @Result(id = true,property = "userId",column = "id"),
            @Result(property = "userName",column = "username"),
            @Result(property = "userTelephone",column = "telephone"),
            @Result(property = "userGender",column = "gender"),
            @Result(property = "userAddress",column = "address"),
            @Result(property = "userBirthday",column = "birthday")
    })
    List<User> findAll();

    @Select("select * from user where username like '%${value}%'")
    @ResultMap("userMap")
    List<User> findUserByName(String username);

}
```

## 多表查询操作

### mybatis注解开发一对一的查询配置

编写`IAccountDao.java`：

```java
public interface IAccountDao {

    /**
     * 查询所有账户，并获取每个账户所属的用户信息
     * @return
     * one = @one注解表示一对一操作，select中配置调用方法，fetchType配置延迟或立即加载
     */
    @Select("select * from account")
    @Results(id = "accountMap",value = {
            @Result(id = true, property = "id", column = "id"),
            @Result(property = "uid", column = "uid"),
            @Result(property = "money", column = "money"),
            @Result(property = "user", column = "uid",
                    one=@One(select="com.hyc.dao.IUserDao.findById",fetchType = FetchType.EAGER))
    })
    List<Account> findAll();
}
```



### Mybatis注解开发一对多的查询配置

编写`IUserDao.java`:

```java
@Select("select * from user")
    @Results(id = "userMap",value = {
            @Result(id = true,property = "userId",column = "id"),
            @Result(property = "userName",column = "username"),
            @Result(property = "userTelephone",column = "telephone"),
            @Result(property = "userGender",column = "gender"),
            @Result(property = "userAddress",column = "address"),
            @Result(property = "userBirthday",column = "birthday"),
            @Result(property = "accounts", column = "id",
                    many = @Many(select="com.hyc.dao.IAccountDao.findAccountByUid",
                            fetchType = FetchType.LAZY))
    })
    List<User> findAll();
```

编写`IAccountDao.java`:

```java
@Select("select * from account where uid=#{uid}")
    List<Account> findAccountByUid(Integer uid);
```



## 二级缓存的配置

```java
@Test
    public void testFindOne(){
        SqlSession session = factory.openSession();;
        IUserDao userDao = session.getMapper(IUserDao.class);;
        User user = userDao.findById(10);
        System.out.println(user);

        session.close();//释放一级缓存

        SqlSession session1 = factory.openSession();
        IUserDao userDao1 = session1.getMapper(IUserDao.class);
        User u1 = userDao1.findById(10);
        System.out.println(u1);
        session1.close();

    }
```

在IUserDao.java中配置注解`@CacheNamespace`

```java
package com.hyc.dao;

import com.hyc.domain.User;
import org.apache.ibatis.annotations.*;
import org.apache.ibatis.mapping.FetchType;

import java.util.List;

/**
 * 用户的持久层接口
 */

@CacheNamespace(blocking = true)
public interface IUserDao {

    /**
     * 查询所有用户，同时获取到用户下所有账户的信息
     * @return
     * 在mybatis中，针对CRUD一共有四个注解：@Select, @Insert, @Update, @Delete
     */
    @Select("select * from user")
    @Results(id = "userMap",value = {
            @Result(id = true,property = "userId",column = "id"),
     ...

}

```

在SqlMapConfig.xml中开启二级缓存（默认已开启）：

```xml
<configuration>

    <properties resource="jdbcConfig.properties"/>

    <!--配置开启二级缓存-->
    <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>

    <typeAliases>
        <package name="com.hyc.domain"/>
    </typeAliases>
  ....
</configuration>
```

