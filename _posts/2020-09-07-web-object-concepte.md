---
title: VO/DTO/DO/PO的区别
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

