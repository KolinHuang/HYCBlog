---
title: Java访问控制
author: Kol Huang
date: 2020-09-12 18:39:00 +0800
categories: [Blogging, java]
tags: [java]
comments: true
math: true

---



![java_访问控制之关键字](/HYCBlog/assets/img/java/java_访问控制之关键字.png)

![java_访问控制之方法](/HYCBlog/assets/img/java/java_访问控制之方法.png)

- 父类引用只能调用父类中定义的方法和变量。
- 对于父类中定义的方法，如果子类中重写了该方法，那么父类类型的引用将会调用子类中的这个方法，这就是动态连接。



还有一种特殊情形，当子类重写了父类的某个方法时，子类的父类引用调用了一个父类的其他方法，这个方法中调用了被重写的父类方法，那么执行的结果会是被重写过后的父类方法，并不会执行原来的父类方法。具体示例如下：

```java
 class Father{
     public void func1(){
         func2();
     }
     //这是父类中的func2()方法，因为下面的子类中重写了该方法所以在父类类型的引用中调用时，这个方法将不再有效
     //取而代之的是将调用子类中重写的func2()方法
     public void func2(){
         System.out.println("AAA");
     }
 }
 class Child extends Father{
     //func1(int i)是对func1()方法的一个重载由于在父类中没有定义这个方法，所以它不能被父类类型的引用调用
     //所以在下面的main方法中child.func1(68)是不对的
     public void func1(int i){
         System.out.println("BBB");
     }
     //func2()重写了父类Father中的func2()方法如果父类类型的引用中调用了func2()方法，那么必然是子类中重写的这个方法
     public void func2(){
         System.out.println("CCC");
     }
 }
 
 public class PolymorphismTest {
     public static void main(String[] args) {
         Father child = new Child();
         child.func1();//打印结果将会是什么？
     }
 }
```

