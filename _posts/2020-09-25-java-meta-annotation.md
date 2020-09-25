---
title: Java的四个元注解
author: Kol Huang
date: 2020-09-25 10:17:00 +0800
categories: [Blogging, java]
tags: [java]
comments: true
math: true
---





* `@Target`

  * 描述注解的使用范围，即被修饰的注解可以用在什么地方。

  * 注解可以用于修饰 packages、types（类、接口、枚举、注解类）、类成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数）

  * ```java
    public enum ElementType {
     
        TYPE, // 类、接口、枚举类
        FIELD, // 成员变量（包括：枚举常量）
        METHOD, // 成员方法
        PARAMETER, // 方法参数
        CONSTRUCTOR, // 构造方法
        LOCAL_VARIABLE, // 局部变量
        ANNOTATION_TYPE, // 注解类
        PACKAGE, // 可用于修饰：包
        TYPE_PARAMETER, // 类型参数，JDK 1.8 新增
        TYPE_USE // 使用类型的任何地方，JDK 1.8 新增
    }
    ```

* `@Retention`

  * 描述注解保留的时间范围，即被描述的注解在它所修饰的类中可以被保留到何时。

  * 用来限定那些被它所注解的注解类在注解到其他类上以后，可被保留到何时，一共有三种策略，定义在RetentionPolicy枚举中

  * ```java
    public enum RetentionPolicy {
     
        SOURCE,    // 源文件保留
        CLASS,       // 编译期保留，默认值
        RUNTIME   // 运行期保留，可通过反射去获取注解信息
    }
    ```

* `@Documented`

  * 描述在使用 javadoc 工具为类生成帮助文档时是否要保留其注解信息。

* `@Inherited`

  * 使被它修饰的注解具有继承性，如果某个类使用了被@Inherited修饰的注解，则其子类将自动具有该注解。

