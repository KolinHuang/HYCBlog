---
title: Mybatis：CRUD
author: Kol Huang
date: 2020-09-04 19:19:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



## 保存操作

在用户Dao接口中定义`saveUser`抽象方法，用于保存用户：

```java
package com.hyc.dao;

import com.hyc.domain.User;
import org.apache.ibatis.annotations.Select;

import java.util.List;

/**
 * 用户的持久层接口
 */
public interface IUserDao {

    /**
     * 查询所有操作
     * @return
     */
//    @Select("select * from user")
//    List<User> findAll();

    void saveUser(User user);
}

```

在`IUserDao.xml`配置文件中配置`<insert>`信息：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.hyc.dao.IUserDao">
    
<!--    配置查询所有-->
    <select id="findAll" resultType="com.hyc.domain.User">
        select * from user
    </select>
<!--		配置保存用户-->
    <insert id="saveUser" parameterType="com.hyc.domain.User">
        insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday});
    </insert>
</mapper>
```

测试（<span style="color:blue">需要提交事务</span>）：

```java
/**
 * mybatis入门案例
 */
public class MybatisTest {
    private InputStream in;
    private SqlSession session;
    private IUserDao userDao;

    @Before
    public void init() throws IOException {
        //1.读取配置文件
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sessionFactory = builder.build(in);
        //3.使用工厂生产SqlSession对象
        session = sessionFactory.openSession();
        //4.使用SqlSession创建Dao接口的代理对象
        userDao = session.getMapper(IUserDao.class);
    }

    @After
    public void destroy() throws IOException {
      	//提交事务
        session.commit();
        //释放资源
        session.close();
        in.close();
    }

    @Test
    public  void testFindAll() {
        //5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }

    }

    /**
     * 测试保存用户
     */
    @Test
    public void testSave() {
        User user = new User();
        user.setUsername("xx");
        user.setAddress("杭州");
        user.setSex("1");
        user.setBirthday(new Date());

        //5.执行保存方法
        userDao.saveUser(user);
    }

}

```



## 更新操作

在用户Dao接口中定义`updateUser`抽象方法，用于更新用户：

```java
    /**
     * 更新操作
     * @param user
     */
    void updateUser(User user);
```

在`IUserDao.xml`配置文件中配置`<update>`信息：

```xml
    <update id="updateUser" parameterType="com.hyc.domain.User">
        update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id=#{id};
    </update>
```

测试：

```java
   	public void testUpdate() {
        User user = new User();
        user.setId(5);
        user.setUsername("xxxx");
        user.setAddress("北京");
        user.setSex("0");
        user.setBirthday(new Date());

        //执行更新方法
        userDao.updateUser(user);

    }
```



## 删除操作

在用户Dao接口中定义`deleteUser`抽象方法，用于更新用户：

```java
    /**
     * 删除操作
     * @param userId
     */
    void deleteUser(Integer userId);
```

在`IUserDao.xml`配置文件中配置`<delete>`信息：

```xml
    <delete id="deleteUser" parameterType="Integer">
        delete from user where id = #{uid}
    </delete>
```

测试：

```java
    /**
     * 测试删除用户
     */
    @Test
    public void testDelete() {

        //执行删除方法
        userDao.deleteUser(5);

    }
```



## 条件查询

在用户Dao接口中定义`findById`抽象方法，用于查询用户：

```java
    /**
     * 根据用户id查询
     * @param userId
     */
    User findById(Integer userId);
```

在`IUserDao.xml`配置文件中配置`<select>`信息：

```xml
    <select id="findById" parameterType="int" resultType="com.hyc.domain.User">
        select * from user where id = #{uid}
    </select>
```

测试：

```java
    /**
     * 测试条件查询
     */
    @Test
    public void testFindById() {

        //执行查询方法
        User user = userDao.findById(2);
        System.out.println(user);

    }
```



## 模糊查询

在用户Dao接口中定义`findByName`抽象方法，用于查询用户：

```java
    /**
     * 根据用户姓名查询
     * @param name
     * @return
     */
    List<User> findByName(String name);
```

在`IUserDao.xml`配置文件中配置`<select>`信息：

```xml
    <select id="findByName" parameterType="String" resultType="com.hyc.domain.User">
        select * from user where username like #{uname}
    </select>
```

测试：

```java
    /**
     * 测试模糊查询
     */
    @Test
    public void testFindByName() {

        //执行查询方法

        List<User> users = userDao.findByName("%li%");
        for (User user : users) {
            System.out.println(user);
        }
    }
```





## 保存操作的细节-获取保存数据的id

修改`IUserDao.xml`配置文件，新增`selectKey`标签，获取插入后的id值，并保存到User实体对象中：

```xml
<insert id="saveUser" parameterType="com.hyc.domain.User">
        <!--keyProperty代表要返回的值名称 order：取值为AFTER代表插入后的行为 resultType代表返回值的类型-->
        <selectKey keyProperty="id" keyColumn="id" order="AFTER" resultType="Integer">
            select last_insert_id();
        </selectKey>
        insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday});

    </insert>
```



## 使用实体类的包装对象作为查询条件

OGNL（Object Graphic Navigation Language）表达式：

* 通过对象的取值方法来获取数据，在写法上把get省略了。
  * 类中写法：user.getUsername();
  * OGNL表达式写法：user.username
  * mybatis中，因为在parameterType中已经提供了属性所属的类，所以此时不需要写对象名，直接写username



修改`IUserDao.xml`配置文件：

```xml
		<select id="findByUserName" parameterType="com.hyc.domain.QueryVo" resultType="com.hyc.domain.User">
        select * from user where username like #{user.username}
    </select>
```



在用户`Dao`接口中定义抽象方法：

```java
/**
     * 根据对象集查询
     * @param vo
     * @return
     */
    List<User> findByUserName(QueryVo vo);
```

定义查询类`QueryVo`：

```java
package com.hyc.domain;

public class QueryVo {
    private User user;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
}
```

测试：

```java
@Test
    public void testFindByUserName(){
        QueryVo vo  = new QueryVo();
        User user = new User();
        user.setUsername("lisi");
        user.setAddress("Hangzhou");
        user.setSex("1");

        vo.setUser(user);
        List<User> users = userDao.findByUserName(vo);
        for (User user1 : users) {
            System.out.println(user1);
        }
    }
```



## 解决实体类属性和数据库列名不对应的两种方式

1. 在查询时起别名：

   ```mysql
   select id as userId, username as userName, address as userAddress... from user;
   ```

2. 在`IUserDao.xml`文件中配置`<resultMap>`：

   ```xml
   <mapper ...>
     <resultMap id="userMap" type "com.hyc.domain.User">
     <!--主键字段的对应-->
     <id property="userId" column="id"></id>
     <!--非主键字段的对应-->
     <result property="userName" column="username"></result>
     <result property="userAddress" column="address"></result>
     <result property="userSex" column="sex"></result>
     <result property="userBirthday" column="birthday"></result>
   	</resultMap>
     
     <!--修改resultType属性为resultMap，id为resultMap配置时的id-->
     <select id="findAll" resultMap="userMap">
       ...
     </select>
     
     ...
   </mapper>
   ```

   



