---
title: Mybatis：环境搭建
author: Kol Huang
date: 2020-09-04 19:17:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---





1. 创建maven工程并导入坐标：

   ```xml
   		<dependencies>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.4.6</version>
           </dependency>
   
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.46</version>
           </dependency>
   
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
   
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
               <scope>test</scope>
           </dependency>
   
       </dependencies>
   
   
   ```

   

2. 创建实体类和dao接口：

   ```java
   /**
    * 实体类
    */
   public class User implements Serializable {
   
       private Integer id;
       private String username;
       private Date birthday;
       private String sex;
       private String address;
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   
       public Date getBirthday() {
           return birthday;
       }
   
       public void setBirthday(Date birthday) {
           this.birthday = birthday;
       }
   
       public String getSex() {
           return sex;
       }
   
       public void setSex(String sex) {
           this.sex = sex;
       }
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   
       @Override
       public String toString() {
           return "User{" +
                   "id=" + id +
                   ", username='" + username + '\'' +
                   ", birthday=" + birthday +
                   ", sex='" + sex + '\'' +
                   ", address='" + address + '\'' +
                   '}';
       }
   }
   
   ```

   ```java
   /**
    * 用户的持久层接口
    */
   public interface IUserDao {
   
       /**
        * 查询所有操作
        * @return
        */
       List<User> findAll();
   }
   
   ```

   

3. 创建mybatis的主配置文件: SqlMapConfig.xml：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org/DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   
   <!--mybatis的主配置文件-->
   <configuration>
   <!--    配置环境-->
       <environments default="mysql">
   <!--        配置mysql的环境-->
           <environment id="mysql">
   <!--            配置事务的类型-->
               <transactionManager type="JDBC"></transactionManager>
   <!--            配置数据源（连接池）-->
               <dataSource type="POOLED">
   <!--                配置连接数据库的4个基本信息-->
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                   <property name="username" value="root"/>
                   <property name="password" value="qazwsxedc7410"/>
               </dataSource>
           </environment>
       </environments>
   
   <!--    指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件-->
       <mappers>
           <mapper resource="com/hyc/dao/IUserDao.xml"/>
       </mappers>
   </configuration>
   ```

   

4. 创建映射配置文件: IDaoUser.xml：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.hyc.dao.IUserDao" resultType="com.hyc.domain.User">
       
   <!--    配置查询所有-->
       <select id="findAll">
           select * from user
       </select>
   </mapper>
   ```

   

   *环境搭建的注意事项：

   1. 创建IUserDao.xml和IUserDao.java时，名称也可以取为IUserMapper.xxx，因为在Mybatis中它把持久层接口名称和映射文件也叫做：Mapper。

   2. mybatis的映射配置文件必须和dao接口的包结构相同。

   3. 映射配置文件的mapper标签namespace属性的取值必须是dao的全限定类名。

   4. 映射配置文件的操作配置，id属性的取值必须dao接口的方法名。

   5. 当我们严格遵循了2、3、4点后，在开发中就无需写dao的实现类。

      