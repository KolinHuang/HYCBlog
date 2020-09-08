---
title: Mybatis：一对一、一对多、多对多操作
author: Kol Huang
date: 2020-09-04 19:24:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



## 完成account的一对一操作



### 通过写account的子类实现

定义`account`子类：

```java
package com.hyc.domain;

public class AccountUser extends Account{
    private String username;
    private String address;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return super.toString()+"       AccountUser{" +
                "username='" + username + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}
```

在`IAccountDao`中定义抽象方法：

```java
package com.hyc.dao;

import com.hyc.domain.Account;
import com.hyc.domain.AccountUser;

import java.util.List;

public interface IAccountDao{

    /**
     * 查询所有账户
     * @return
     */
    List<Account> findAll();

    /**
     * 查询所有账户，同时还要获取到当前账户的所属用户信息
     * @return
     */
    List<AccountUser> findAllAccount();
}
```

在`IAccountDao.xml`中配置查询信息：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.hyc.dao.IAccountDao">

<!--    配置查询-->
    <select id="findAll" resultType="Account">
        select * from account
    </select>

    <select id="findAllAccount" resultType="AccountUser">
        select u.*,a.id,a.uid,a.money from account as a,user as u where a.uid=u.id
    </select>
</mapper>
```

测试：

```java
    @Test
    public void testFindAllAccount(){
        List<AccountUser> aus = accountDao.findAllAccount();
        for (AccountUser au : aus) {
            System.out.println(au);
        }
    }
```



### 通过建立实体类关系的方式实现

首先在`Account`实体类中建立`User`类的引用，因为从表实体应该包含一个主表实体的对象引用：

```java
private User user;
public User getUser() {
  return user;
}

public void setUser(User user) {
  this.user = user;
}
```

再在`IAccountDao.xml`文件中配置映射：

```xml
    <resultMap id="accountUserMap" type="account">
<!--        主键对应-->
        <id property="id" column="aid"></id>
<!--        非主键对应-->
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
<!--        一对一的关系映射，配置封装user的内容-->
        <association property="user" column="uid" javaType="user">
            <result property="id" column="id"></result>
            <result property="username" column="username"></result>
            <result property="telephone" column="telephone"></result>
            <result property="birthday" column="birthday"></result>
            <result property="gender" column="gender"></result>
            <result property="address" column="address"></result>

        </association>
    </resultMap>

<!--    配置查询-->
    <select id="findAll" resultMap="accountUserMap">
        select u.*,a.id as aid,a.uid,a.money from account as a,user as u where u.id = a.uid;
    </select>
```

测试：

```java
@Test
    public void testFindAll() {
        //5.使用代理对象执行方法
        List<Account> accounts = accountDao.findAll();
        for (Account account : accounts) {
            System.out.println(account);
            System.out.println(account.getUser());
        }
    }
```





## 完成user的一对多查询操作

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.hyc.dao.IUserDao">

    <resultMap id="userMap" type="user">
        <!--        主键对应-->
        <id property="id" column="id"></id>
        <!--        非主键对应-->
        <result property="username" column="username"></result>
        <result property="telephone" column="telephone"></result>
        <result property="birthday" column="birthday"></result>
        <result property="gender" column="gender"></result>
        <result property="address" column="address"></result>
<!--        配置user对象中accounts集合的映射-->
        <collection property="accounts" ofType="account">
            <result property="id" column="aid"></result>
            <result property="uid" column="uid"></result>
            <result property="money" column="money"></result>

        </collection>
    </resultMap>

<!--    配置查询所有-->
    <select id="findAll" resultMap="userMap">
        select u.*,a.id as aid,a.uid,a.money from user as u left outer join account as a on a.uid=u.id
    </select>


</mapper>
```

在`user`实体类中定义`account`的集合对象：

```java
    private List<Account> accounts;

    public List<Account> getAccounts() {
        return accounts;
    }

    public void setAccounts(List<Account> accounts) {
        this.accounts = accounts;
    }
```



测试：

```java
		@Test
    public void testFindAll() {
        //5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
            System.out.println(user.getAccounts());
    }
```

​	



## mybatis的多对多操作



### 查询角色，获取角色所属所有用户

建立角色实体类：

```java
package com.hyc.domain;

import java.io.Serializable;
import java.util.List;

public class Role implements Serializable {
    private Integer roleId;
    private String roleName;
    private String roleDesc;

    private List<User> users;

    public List<User> getUsers() {
        return users;
    }

    public void setUsers(List<User> users) {
        this.users = users;
    }

    public Integer getRoleId() {
        return roleId;
    }

    public void setRoleId(Integer roleId) {
        this.roleId = roleId;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDesc() {
        return roleDesc;
    }

    public void setRoleDesc(String roleDesc) {
        this.roleDesc = roleDesc;
    }

    @Override
    public String toString() {
        return "Role{" +
                "roleId=" + roleId +
                ", roleName='" + roleName + '\'' +
                ", roleDesc='" + roleDesc + '\'' +
                '}';
    }
}

```

创建`IRoleDao.java`接口:

```java
package com.hyc.dao;

import com.hyc.domain.Role;

import java.util.List;

public interface IRoleDao {
    /**
     * 查询所有角色
     * @return
     */
    List<Role> findAll();
}

```

创建`IRoleDao.xml`配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.hyc.dao.IRoleDao">

    <resultMap id="roleMap" type="role">
        <!--        主键对应-->
        <id property="roleId" column="rid"></id>
        <!--        非主键对应-->
        <result property="roleName" column="role_name"></result>
        <result property="roleDesc" column="role_desc"></result>
        <!--        配置user对象中accounts集合的映射-->
        <collection property="users" ofType="user">
            <result property="id" column="id"></result>
            <result property="username" column="username"></result>
            <result property="telephone" column="telephone"></result>
            <result property="birthday" column="birthday"></result>
            <result property="gender" column="gender"></result>
            <result property="address" column="address"></result>
        </collection>
    </resultMap>

    <!--    配置查询所有-->
    <select id="findAll" resultMap="roleMap">
        select u.*,r.id as rid, r.role_name,r.role_desc from role as r
        left outer join user_role as ur on r.ID=ur.RID
        left outer join user as u on  u.id=ur.UID;
    </select>
</mapper>
```

测试：

```java
package com.hyc.test;


import com.hyc.dao.IRoleDao;
import com.hyc.dao.IUserDao;
import com.hyc.domain.Role;
import com.hyc.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * mybatis入门案例
 */
public class RoleTest {
    private InputStream in;
    private SqlSession session;
    private IRoleDao roleDao;

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
        roleDao = session.getMapper(IRoleDao.class);
    }

    @After
    public void destroy() throws IOException {
        session.commit();
        //释放资源
        session.close();
        in.close();
    }

    @Test
    public void testFindAll() {
        //5.使用代理对象执行方法
        List<Role> roles = roleDao.findAll();
        for (Role role : roles) {
            System.out.println(role);
            System.out.println(role.getUsers());
        }
    }
}
```



### 查询用户，获取用户所有角色

修改User实体类，添加角色联系：

```java
    private List<Role> roles;

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }
```

修改`IUserDao.xml`配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.hyc.dao.IUserDao">

    <resultMap id="userMap" type="user">
        <!--        主键对应-->
        <id property="id" column="id"></id>
        <!--        非主键对应-->
        <result property="username" column="username"></result>
        <result property="telephone" column="telephone"></result>
        <result property="birthday" column="birthday"></result>
        <result property="gender" column="gender"></result>
        <result property="address" column="address"></result>
<!--        配置user对象中roles集合的映射-->
        <collection property="roles" ofType="role">
            <result property="id" column="rid"></result>
            <result property="roleName" column="role_name"></result>
            <result property="roleDesc" column="role_desc"></result>

        </collection>
    </resultMap>

<!--    配置查询所有-->
    <select id="findAll" resultMap="userMap">
        select u.*,r.ROLE_DESC,ROLE_NAME from user u
         left outer join user_role ur on u.id = ur.UID
         left outer join role r on r.ID=ur.RID;
    </select>

    <select id="findById" parameterType="int" resultType="User">
        select * from user where id = #{uid}
    </select>


</mapper>
```

测试：

```java
    @Test
    public void testFindAll() {
        //5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
            System.out.println(user.getRoles());
        }
    }
}

```

