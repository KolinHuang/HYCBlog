---
title: Mybatis：入门
author: Kol Huang
date: 2020-09-04 19:18:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---

## 入门案例

```java
public class MybatisTest {
    public static void main(String[] args) throws IOException {
        //1.读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sessionFactory = builder.build(in);
        //3.使用工厂生产SqlSession对象
        SqlSession session = sessionFactory.openSession();
        //4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);
        //5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }
        //6.释放资源
        session.close();
        in.close();
    }
}
```

## 案例分析

1. 在读配置文件或读文件的时候，一般不用绝对路径和相对路径，即：

   ```markdown
   绝对路径：d:/xxx/xxx.xml
   相对路径：src/java/main/xxx.xml
   ```

   一般使用以下两种方法：

   ```markdown
   1. 使用类加载器，它只能读取类路径的配置文件。
   2. 使用ServletContext对象的getRealPath()，能够获得当前项目的部署路径。
   ```

2. 创建工厂,`mybatis`使用了构建者模式，`builder`就是构建者。

   ```markdown
   优势：把对象的创建细节隐藏，使使用者直接调用方法即可拿到对象。
   ```

3. 生产`SqlSession`使用了工厂模式。

   ```markdown
   优势：解耦，即降低类之间的依赖关系。
   ```

4. 创建`Dao`接口实现类使用了代理模式。

   ```markdown
   优势：不修改源码的基础上对已有方法增强。
   ```



## mybatis基于注解的入门案例

把`IUserDao.xml`移除，在`Dao`接口的方法上使用`@select`注解，并且制定SQL语句。

```java
/**
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有操作
     * @return
     */
    @Select("select * from user")
    List<User> findAll();
}
```

同时需要在`SqlMapConfig.xml`中的`mapper`配置中，使用`class`属性指定`Dao`接口的全限定类名。

```xml
    <mappers>
<!--        <mapper resource="com/hyc/dao/IUserDao.xml"/>-->
        <mapper class="com.hyc.dao.IUserDao"/>
    </mappers>
```



* `Mybatis`是支持写`dao`实现类的，但是在开发中不会去写。

* `Mybatis`使用配置文件中的`namespace`属性以及id属性定位sql语句：

  ```xml
  <mapper namespace="com.hyc.dao.IUserDao">
      
  <!--    配置查询所有-->
      <select id="findAll" resultType="com.hyc.domain.User">
          select * from user
      </select>
  ```

  ```java
  statement = "com.hyc.dao.IUserDao.findAll"
  ```

  

