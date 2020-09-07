---
title: Spring：程序的耦合
author: Kol Huang
date: 2020-09-05 21:39:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



- # 程序的耦合和解耦思路

  ```java
  package com.yucaihuang.jdbc;
  
  import java.sql.*;
  
  /**
   * 程序耦合
   *  耦合：程序间的依赖关系，包括：类之间的依赖、方法间的依赖
   *  解耦：降低程序间的依赖关系
   *  实际开发中，应该做到：编译期不依赖，运行时才依赖
   *  解耦的思路：
   *      第一步：使用反射来创建对象，而避免使用new关键字
   *      第二步：通过读取配置文件，来获取要创建的对象的全限定类名
   */
  public class JdbcDemo1 {
      public static void main(String[] args) throws SQLException, ClassNotFoundException {
          //1.注册驱动
  //        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
          Class.forName("com.mysql.jdbc.Driver");
          //2.获取连接
          Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/eesy_mybatis",
                  "root", "qazwsxedc7410");
          //3.获取预处理对象
          PreparedStatement stmt = conn.prepareStatement("select * from account");
          //4.遍历结果集
          ResultSet rs = stmt.executeQuery();
          while(rs.next()){
              System.out.println(rs.getString("money"));
          }
          rs.close();
          stmt.close();
          conn.close();
      }
  }
  ```

  

  

  

  

  