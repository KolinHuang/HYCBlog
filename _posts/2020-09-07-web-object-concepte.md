---
title: VO/DTO/DO/PO/POJO/JavaBean的区别
author: Kol Huang
date: 2020-09-09 19:25:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true

---



> VO

VO（View Object）：视图对象，用于展示层，作用是把某个指定wb页面（或组件）的所有数据封装起来。



> DTO

DTO（Data Transfer Object）：数据传输对象，这个概念来源于J2EE的设计模式，原来的目的是为了EJB的分布式应用提供粗粒度的数据实体，以减少分布式调用的次数，从而提高分布式调用的性能和降低网络负载。也可泛指为展示层与服务层之间的数据传输对象。



> DO

DO（Domain Object）：领域对象，就是从现实世界中抽象出来的有形或无形的业务实体。



> PO

PO（Persistent Object）：持久化对象，它跟持久层（通常是关系型数据库）的数据结构形成一一对应的映射关系，如果持久层是关系型数据库，那么数据表中的每个字段（或若干个）就对应一个PO的一个（或若干个）属性。

![img](https://user-gold-cdn.xitu.io/2019/8/22/16cb73864f045c4a?imageslim)

- 用户发出请求（可能是填写表单），表单的数据在展示层被匹配为VO。
- 展示层把VO转换为服务层对应方法所要求的DTO，传送给服务层。
- 服务层首先根据DTO的数据构造（或重建）一个DO，调用DO的业务方法完成具体业务。
- 服务层把DO转换为持久层对应的PO（可以使用ORM工具，也可以不用），调用持久层的持久化方法，把PO传递给它，完成持久化操作。
- 对于一个逆向操作，如读取数据，也是用类似的方式转换和传递，略。

参考：[浅析VO、DTO、DO、PO的概念、区别和用处](https://juejin.im/post/6844903921538826253)



> POJO

"Plain Ordinary Java Object"，简单普通的java对象。主要用来指代那些没有遵循特定的java对象模型，约定或者框架的对象。

POJO的内在含义是指那些:

* 有一些private的参数作为对象的属性，然后针对每一个参数定义get和set方法访问的接口。
* 没有从任何类继承、也没有实现任何接口，更没有被其它框架侵入的java对象。



> JavaBean

JavaBean 是一种JAVA语言写成的可重用组件。JavaBean符合一定规范编写的Java类，不是一种技术，而是一种规范。大家针对这种规范，总结了很多开发技巧、工具函数。符合这种规范的类，可以被其它的程序员或者框架使用。它的方法命名，构造及行为必须符合特定的约定：

* 所有属性为private。
* 这个类必须有一个公共的缺省构造函数。即是提供无参数的构造器。
* 这个类的属性使用getter和setter来访问，其他方法遵从标准命名规范。
* 这个类应是可序列化的。实现serializable接口。

因为这些要求主要是靠约定而不是靠实现接口，所以许多开发者把JavaBean看作遵从特定命名约定的POJO。



**POJO和JavaBean的区别**

* POJO其实是比javabean更纯净的简单类或接口。POJO严格地遵守简单对象的概念，而一些JavaBean中往往会封装一些简单逻辑。
* POJO主要用于数据的临时传递，它只能装载数据， 作为数据存储的载体，而不具有业务逻辑处理的能力。
* Javabean虽然数据的获取与POJO一样，但是javabean当中可以有其它的方法。

