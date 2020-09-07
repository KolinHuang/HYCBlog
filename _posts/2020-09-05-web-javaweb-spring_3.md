---
title: Spring：工厂模式解耦
author: Kol Huang
date: 2020-09-05 21:40:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



## 问题代码

`IAccountDao.java`:

```java
/**
 * 账户的持久层接口
 */
public interface IAccountDao {
    void saveAccount();
}

```

`AccountDaoImpl.java`:

```java
/**
 * 账户的持久层实现类
 */
public class AccountDaoImpl implements IAccountDao {
    public void saveAccount() {
        System.out.println("账户已保存");
    }
}
```

`IAccountService.java`:

```java
/**
 * 账户业务层接口
 */
public interface IAccountService {

    void saveAccount();
}
```

`AccountServiceImpl.java`:

```java
//账户的业务层实现类
public class AccountServiceImpl implements IAccountService {
		
  	//使用new关键字使得耦合性增强
    private IAccountDao accountDao = new AccountDaoImpl();

    public void saveAccount() {
        accountDao.saveAccount();
    }
}
```

`Client.java`:

```java
/**
 * 模拟一个表现层，用于调用业务层
 */
public class Client {
    public static void main(String[] args) {
        IAccountService as = new AccountServiceImpl();
        as.saveAccount();

    }
}
```



## 工厂模式解耦（多例）

创建工厂类`BeanFactory`:

```java
/**
 * 一个创建Bean对象的工厂
 *
 * Bean：在计算机英语中，有重用组件的含义
 * JavaBean: 用Java语言编写的可重用组件
 *      javaBean > 实体类
 *
 * 这个工厂就是创建Service 和Dao对象的。
 *  第一步：需要一个配置文件来配置Service和Dao
 *      配置内容：唯一标识 = 全限定类名 （key=value）
 *  第二步：通过读取配置文件中配置的内容，反射创建对象
 *  配置文件可以是xml，也可以是properties
 */
public class BeanFactory {
    private static Properties props;
    //使用静态代码块为Properties对象赋值
    static {
        try {
            //实例化对象
            props = new Properties();
            //获取properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
        }catch (Exception e){
            throw new ExceptionInInitializerError("初始化properties失败！");
        }
    }

    /**
     * 根据Bean的名称获取bean对象
     * @param beanName
     * @return
     */
    public static Object getBean(String beanName){
        Object bean = null;
        try {
            //获取类文件路径
            String beanPath = props.getProperty(beanName);
            //用反射获取对象
            bean = Class.forName(beanPath).newInstance();//每次都会调用默认构造函数创建对象
        }catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        return bean;
    }
}

```

在`AccountServiceImpl.java`中用工厂类创建账户Dao实现类：

```java
//账户的业务层实现类
public class AccountServiceImpl implements IAccountService {

//    private IAccountDao accountDao = new AccountDaoImpl();
    private IAccountDao accountDao = (IAccountDao) BeanFactory.getBean("accountDao");
    public void saveAccount() {
        accountDao.saveAccount();
    }
}
```

在`Client.java`中用工厂类创建账户service实现类：

```java
/**
 * 模拟一个表现层，用于调用业务层
 */
public class Client {
    public static void main(String[] args) {
//        IAccountService as = new AccountServiceImpl();
        IAccountService as = (IAccountService) BeanFactory.getBean("accountService");
        as.saveAccount();
    }
}
```

在resource资源目录下创建bean.properties配置文件：

```properties
accountService=com.yucaihuang.service.impl.AccountServiceImpl
accountDao=com.yucaihuang.dao.impl.AccountDaoImpl
```

**这样就能在工厂类中，用配置文件bean.properties中定义的实体类全限定类名通过反射获取对象，并返回给业务层实现类，来调用持久层对象。再用同样的方法，将业务层实现类对象返回给表现层实体类，来调用业务层对象。**



## 工厂模式解耦（单例）

修改工厂类：

```java
public class BeanFactory {
    private static Properties props;

    //定义一个Map，用于存放我们要创建的对象，我们把它称之为容器
    private static Map<String,Object> beans;


    //使用静态代码块为Properties对象赋值
    static {
        try {
            //实例化对象
            props = new Properties();
            //获取properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
          
          
            //实例化容器
            beans = new HashMap<String, Object>();
            //取出配置文件中所有的key
            Enumeration keys = props.keys();
            while(keys.hasMoreElements()){
                //取出每个key
                String key = keys.nextElement().toString();
                //根据key获取value
                String beanPath = props.getProperty(key);
                //反射创建对象
                Object value = Class.forName(beanPath).newInstance();
                //存入容器
                beans.put(key,value);
              
              
            }
//            for (Map.Entry<String, Object> stringObjectEntry : beans.entrySet()) {
//                System.out.println(stringObjectEntry);
//            }
        }catch (Exception e){
            throw new ExceptionInInitializerError("初始化properties失败！");
        }
    }

    /**
     * 根据Bean的名称获取bean对象
     * @param beanName
     * @return
     */
    public static Object getBean(String beanName){
        Object bean = beans.get(beanName);

        return bean;
    }
}

```





