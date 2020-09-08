---
title: Spring：AOP（面向切面编程）
author: Kol Huang
date: 2020-09-05 21:42:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---





AOP：Aspect Oriented Programming，意为面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发效率。

简单的说，就是AOP把程序中重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的基础上，对已有的方法进行增强。



## 事务控制下的转账功能实现

定义和事物管理相关的工具类：

```java
/**
 * 和事务管理相关的工具类，它包含了：开启事务，提交事务，回滚事务和释放连接
 */
@Component("transactionManager")
public class TransactionManager {

    @Autowired
    private ConnectionUtils connectionUtils;

    public void beginTransaction(){
        try{
            connectionUtils.getConnection().setAutoCommit(false);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
    public void commit(){
        try{
            connectionUtils.getConnection().commit();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
    public void rollback(){
        try{
            connectionUtils.getConnection().rollback();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 释放连接
     */
    public void release(){
        try{
            connectionUtils.getConnection().close();//  放回连接池
            //连接与线程解绑
            connectionUtils.removeConnection();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

}
```

定义连接的工具类，用于从数据源中获取一个连接，并且实现和线程的绑定：

```java
/**
 * 连接的工具类，用于从数据源中获取一个连接，并且实现和线程的绑定
 */
@Component("connectionUtils")
public class ConnectionUtils {

    private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

    @Autowired
    private DataSource dataSource;

    public Connection getConnection(){
        try{
            //先从ThreadLocal上获取
            Connection conn = tl.get();
            if(conn == null){
                //从数据源中获取一个连接，并且存入ThreadLocal中
                conn = dataSource.getConnection();
                tl.set(conn);
            }

            return conn;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     * 把线程和连接解绑
     */
    public void removeConnection(){
        tl.remove();
    }
}
```

在accountDao的实现类中使用获取连接工具类获取数据库连接，并将参数传给runner用于在持久层进行数据库查询操作：

```java
public class AccountDaoImpl implements IAccountDao {

  @Autowired
  private QueryRunner runner;

  @Autowired
  private ConnectionUtils connectionUtils;


  public List<Account> findAllAccount() {
    try {
      return runner.query(connectionUtils.getConnection(),"select * from account",new BeanListHandler<Account>(Account.class));
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public Account findAccountById(Integer id) {
    try {
      return runner.query(connectionUtils.getConnection(),"select * from account where id = ?",new BeanHandler<Account>(Account.class),id);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void saveAccount(Account account) {
    try {
      runner.update(connectionUtils.getConnection(),"insert into account(name,money)values(?,?)",account.getName(),account.getMoney());
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void updateAccount(Account account) {
    try {
      runner.update(connectionUtils.getConnection(),"update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void deleteAccount(Integer id) {
    try {
      runner.update(connectionUtils.getConnection(),"delete from account where id=?",id);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public Account findAccountByName(String name) {
    try {
      List<Account> accounts =  runner.query(connectionUtils.getConnection(),"select * from account where name = ?",new BeanListHandler<Account>(Account.class),name);
      if(accounts == null || accounts.size() == 0){
        return null;
      }
      if(accounts.size() > 1)
        throw new RuntimeException("数据查询错误");

      return accounts.get(0);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }
}
```

在AccountService实现类中利用事务管理器实现在业务层管理事务：

```java
/**
 * 账户的业务层实现类
 *  事务控制应该都在业务层
 *
 */

@Service("accountService")
public class AccountServiceImpl implements IAccountService {


    @Autowired
    private IAccountDao accountDao;

    @Autowired
    private TransactionManager transactionManager;


//    public void setAccountDao(IAccountDao accountDao) {
//        this.accountDao = accountDao;
//    }

    public List<Account> findAllAccount() {
        try{
            //开启事务
            transactionManager.beginTransaction();
            //执行操作
            List<Account> accounts = accountDao.findAllAccount();
            //提交事务
            transactionManager.commit();
            //返回结果
            return accounts;
        }catch(Exception e){
            //回滚操作
            transactionManager.rollback();
            throw new RuntimeException(e);
        }finally {
            //释放连接
            transactionManager.release();
        }

    }

    public Account findAccountById(Integer id) {
        try{
            //开启事务
            //执行操作
            //提交事务
            //返回结果
        }catch(Exception e){
            //回滚操作
        }finally {
            //释放连接
        }
        return accountDao.findAccountById(id);
    }

    public void saveAccount(Account account) {
        try{
            //开启事务
            //执行操作
            //提交事务
            //返回结果
        }catch(Exception e){
            //回滚操作
        }finally {
            //释放连接
        }
        accountDao.saveAccount(account);
    }

    public void updateAccount(Account account) {
        try{
            //开启事务
            //执行操作
            //提交事务
            //返回结果
        }catch(Exception e){
            //回滚操作
        }finally {
            //释放连接
        }
        accountDao.updateAccount(account);
    }

    public void deleteAccount(Integer id) {
        try{
            //开启事务
            //执行操作
            //提交事务
            //返回结果
        }catch(Exception e){
            //回滚操作
        }finally {
            //释放连接
        }
        accountDao.deleteAccount(id);
    }

    public Account findAccountByName(String name) {
        try{
            //开启事务
            transactionManager.beginTransaction();
            //执行操作
            Account account = accountDao.findAccountByName(name);
            //提交事务
            transactionManager.commit();
            //返回结果
            return account;
        }catch(Exception e){
            //回滚操作
            transactionManager.rollback();
            throw new RuntimeException(e);
        }finally {
            //释放连接
            transactionManager.release();
        }
    }

    public void transfer(String sourceName, String targetName, Float money) {
        //需要使用ThreadLocal对象把Connection和当前线程绑定，从而使一个线程中只有一个能控制事务的对象

        try{
            //开启事务
            transactionManager.beginTransaction();
            //执行操作
            //1. 根据名称查询转出账户
            Account sourceAccount = accountDao.findAccountByName(sourceName);
            //2. 根据名称查询转入账户
            Account targetAccount = accountDao.findAccountByName(targetName);
            //3. 转出账户减钱
            sourceAccount.setMoney(sourceAccount.getMoney() - money);
            //4. 转入账户加钱
            targetAccount.setMoney(targetAccount.getMoney() + money);
            //5. 更新转出账户
            accountDao.updateAccount(sourceAccount);
//            int i = 1/0;
            //6. 更新转入账户
            accountDao.updateAccount(targetAccount);
            //提交事务
            transactionManager.commit();

        }catch(Exception e){
            //回滚操作
            transactionManager.rollback();
            e.printStackTrace();
        }finally {
            //释放连接
            transactionManager.release();
        }
    }
}
```

为了在进行数据库操作时，保证操作的原子性，需要执行事务控制。



## 基于接口的动态代理

```markdown
* 动态代理：
		特点：字节码随用随创建，随用随加载
		作用：不修改源码的基础上对方法增强
		分类：基于接口的动态代理、基于子类的动态代理
		基于接口的动态代理：
			涉及的类：Proxy
			提供者：JDK官方
			如何创建代理对象：使用Proxy类中的newProxyInstance方法
			创建代理对象的要求：被代理类最少实现一个接口，如果没有则不能使用
			newProxyInstance的参数：
					ClassLoader:用于加载代理对象字节码的，和被代理对象使用相同的类加载器。固定写法
					Class[]:用于让代理对象和被代理对象有相同的方法。固定写法
					InvocationHandler:用于提供增强的代码，让我们写如何代理。一般都是写一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的。此接口的实现类都是谁用谁写。
```

```java
public class Client {
    public static void main(String[] args) {
        final Producer producer = new Producer();
         IProducer proxyProducer = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(),
                producer.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * 执行被代理对象的任何接口方法都会经过该方法
                     * @param proxy     代理对象的引用
                     * @param method    当前执行的方法
                     * @param args      当前执行方法所需的参数
                     * @return 和被代理对象方法有相同的返回值
                     * @throws Throwable
                     */
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
												//增强的代码
                        Object returnValue = null;
                        
                        //判断当前方法是不是销售
                        if ("saleProduct".equals(method.getName())) {
                          	//获取方法执行的参数
                        		Float money = (Float) args[0];
                            returnValue = method.invoke(producer, money * 0.8f);
                        }
                        return returnValue;
                    }
                });
         proxyProducer.saleProduct(10000f);

    }
}
```

被代理对象所属类：

```java
/**
 * 一个生产者
 */
public class Producer implements IProducer {
    /**
     * 销售
     * @param money
     */
    public void saleProduct(float money){
        System.out.println("销售产品，并拿到钱："+money);
    }
    /**
     * 售后
     * @param money
     */
    public void afterService(float money){
        System.out.println("提供售后服务，并拿到钱："+money);
    }
}
```

接口：

```java
/**
 * 对生产厂家要求的接口
 */
public interface IProducer {
    /**
     * 销售
     * @param money
     */
    public void saleProduct(float money);

    /**
     * 售后
     * @param money
     */
    public void afterService(float money);
}
```

使用`Proxy`类的`newProxyInstance`方法创建了代理对象`proxyProducer`，在参数中配置信息，使得代理对象也拥有被代理对象`Producer`的所有方法（`saleProduct()`），并通过`invoke()`方法对被代理对象的方法进行增强。



## 基于子类的动态代理

```markdown
基于子类的动态代理：
			涉及的类：Enhancer
			提供者：第三方cglib库
			如何创建代理对象：使用Enhancer类中的create法
			创建代理对象的要求：被代理类不能是最终类
			create的参数：
					Class:用于指定被代理对象的字节码
					Callback:用于提供增强的代码。一般写的都是该接口的子接口实现类，MethodInterceptor
```

```java
package com.yucaihuang.cglib;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class Client {
    public static void main(String[] args) {
        final Producer producer = new Producer();

        Producer cglibProducer = (Producer) Enhancer.create(producer.getClass(), new MethodInterceptor() {
            /**
             * 执行被代理对象的任何方法都会经过此方法
             * @param o         代理对象的引用
             * @param method    当前执行的方法
             * @param objects   当前执行方法所需的参数
             * 以上三个参数和基于接口的动态代理中invoke方法的参数是一样的。
             * @param methodProxy   当前执行方法的代理对象
             * @return 和被代理对象方法有相同的返回值
             * @throws Throwable
             */
            public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                //增强的代码
                Object returnValue = null;

                //判断当前方法是不是销售
                if ("saleProduct".equals(method.getName())) {
                    //获取方法执行的参数
                    Float money = (Float) objects[0];
                    returnValue = method.invoke(producer, money * 0.7f);
                }
                return returnValue;
            }
        });
        cglibProducer.saleProduct(10000f);
    }
}
```

## 使用动态代理实现事务控制

持久层实现类：

```java
public class AccountDaoImpl implements IAccountDao {

  private QueryRunner runner;

  private ConnectionUtils connectionUtils;

  public void setRunner(QueryRunner runner) {
    this.runner = runner;
  }

  public void setConnectionUtils(ConnectionUtils connectionUtils) {
    this.connectionUtils = connectionUtils;
  }

  public List<Account> findAllAccount() {
    try {
      return runner.query(connectionUtils.getConnection(),"select * from account",new BeanListHandler<Account>(Account.class));
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public Account findAccountById(Integer id) {
    try {
      return runner.query(connectionUtils.getConnection(),"select * from account where id = ?",new BeanHandler<Account>(Account.class),id);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void saveAccount(Account account) {
    try {
      runner.update(connectionUtils.getConnection(),"insert into account(name,money)values(?,?)",account.getName(),account.getMoney());
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void updateAccount(Account account) {
    try {
      runner.update(connectionUtils.getConnection(),"update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void deleteAccount(Integer id) {
    try {
      runner.update(connectionUtils.getConnection(),"delete from account where id=?",id);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public Account findAccountByName(String name) {
    try {
      List<Account> accounts =  runner.query(connectionUtils.getConnection(),"select * from account where name = ?",new BeanListHandler<Account>(Account.class),name);
      if(accounts == null || accounts.size() == 0){
        return null;
      }
      if(accounts.size() > 1)
        throw new RuntimeException("数据查询错误");

      return accounts.get(0);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }
}

```



业务层实现类：事务控制从业务层中移除，放入代理对象中

```java
public class AccountServiceImpl implements IAccountService {


    private IAccountDao accountDao;


    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }


    public List<Account> findAllAccount() {

        return accountDao.findAllAccount();

    }

    public Account findAccountById(Integer id) {
        return accountDao.findAccountById(id);
    }

    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }

    public void deleteAccount(Integer id) {
        accountDao.deleteAccount(id);
    }

    public Account findAccountByName(String name) {
        return accountDao.findAccountByName(name);


    }

    public void transfer(String sourceName, String targetName, Float money) {



        //1. 根据名称查询转出账户
        Account sourceAccount = accountDao.findAccountByName(sourceName);
        //2. 根据名称查询转入账户
        Account targetAccount = accountDao.findAccountByName(targetName);
        //3. 转出账户减钱
        sourceAccount.setMoney(sourceAccount.getMoney() - money);
        //4. 转入账户加钱
        targetAccount.setMoney(targetAccount.getMoney() + money);
        //5. 更新转出账户
        accountDao.updateAccount(sourceAccount);
        int i = 1/0;
        //6. 更新转入账户
        accountDao.updateAccount(targetAccount);

    }
}
```

编写用于创建service代理对象的工厂类，该代理对象对service对象的各个方法进行增强，实现事务控制：

```java
/**
 * 用于创建service的代理对象的工厂
 */
public class BeanFactory {
    private IAccountService accountService;

    private TransactionManager transactionManager;

    public void setAccountService(IAccountService accountService) {
        this.accountService = accountService;
    }

    public void setTransactionManager(TransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    /**
     * 获取Service代理对象
     * @return
     */
    public IAccountService getAccountService(){
        IAccountService proxyAccountService = (IAccountService) Proxy.newProxyInstance(accountService.getClass().getClassLoader(),
                accountService.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * 添加事务的支持
                     * @param proxy
                     * @param method
                     * @param args
                     * @return
                     * @throws Throwable
                     */
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        Object rtValue = null;
                        try{
                            //开启事务
                            transactionManager.beginTransaction();
                            //执行操作
                            rtValue = method.invoke(accountService,args);
                            //提交事务
                            transactionManager.commit();
                            //返回结果
                            return rtValue;
                        }catch(Exception e){
                            //回滚操作
                            transactionManager.rollback();
                            throw new RuntimeException(e);
                        }finally {
                            //释放连接
                            transactionManager.release();
                        }
                    }
                });

        return proxyAccountService;
    }
}
```

测试：

```java
/**
 * 使用Junit单元测试进行操作
 */
//
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class AccountServiceTest {

    @Autowired
    @Qualifier("proxyAccountService")
    private IAccountService as;

    @Test
     public void testFindAccountByName(){
        Account account = as.findAccountByName("aaa");
        System.out.println(account);
    }

    @Test
    public void testTransfer(){
        as.transfer("aaa","bbb",100.0f);
    }
}
```

Bean.xml的配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">



<!--    配置数据源-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!--        连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"/>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
        <property name="user" value="root"/>
        <property name="password" value="qazwsxedc7410"/>
    </bean>

  	<bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>
  
    <bean id="accountDao" class="com.yucaihuang.dao.impl.AccountDaoImpl">
        <property name="runner" ref="runner"/>
        <property name="connectionUtils" ref="connectionUtils"/>
    </bean>
  
    <!--    配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
    </bean>
  
    <bean id="connectionUtils" class="com.yucaihuang.utils.ConnectionUtils">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <bean id="beanFactory" class="com.yucaihuang.factory.BeanFactory">
        <property name="transactionManager" ref="transactionManager"/>
        <property name="accountService" ref="accountService"/>
    </bean>

    <bean id="transactionManager" class="com.yucaihuang.utils.TransactionManager">
        <property name="connectionUtils" ref="connectionUtils"/>
    </bean>

    <bean id="proxyAccountService" factory-bean="beanFactory" factory-method="getAccountService"></bean>
  
</beans>

```



## Spring中的AOP



### 相关术语

```markdown
* Joinpoint(连接点)：
		指那些被拦截到的点。在Spring中，这些点指的是方法，因为spring只支持方法类型的连接点。
* Pointcut(切入点)：
		指我们要对哪些Joinpoint进行拦截的定义。
*	Advice(通知/增强)：
		拦截到Joinpoint之后所要做的事情就是通知。
		通知的类型：前置通知，后置通知，异常通知，最终通知，环绕通知。
* Introduction(引介)：
		是一种特殊的通知，在不修改类代码的前提下，Introduction可以在运行期为类动态地添加一些方法或Field
* Target(目标对象)：
		代理的目标对象
* Weaving(织入)：
		是指把增强应用到目标对象来创建新的代理对象的过程。
		Spring采用动态代理织入，而AspectJ采用编译器和类装载期织入。
* Proxy(代理)：
		一个类被AOP织入增强后，就产生了一个结果代理类
* Aspect(切面)：
		是切入点和通知(引介)的结合
```



### Spring基于xml的AOP配置

```markdown
* spring中基于XML的AOP配置步骤：
		1、把通知Bean也交给spring来管理
		2、使用aop:config标签表明开始AOP的配置
		3、使用aop:aspect标签表明配置切面
				id属性：是给切面提供一个唯一的标识
				ref属性：是指定通知类bean的id
		4、在aop:aspect标签的内部使用对应标签来配置通知的类型
				前置通知：aop:before标签
						method属性：用于指定Logger类中哪个方法是前置通知
						pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强
							
				后置通知：aop:after标签
切入点表达式的写法：
    关键字：execution(表达式)
    表达式： 访问修饰符 返回值 包名.类名.方法名(参数列表)
    例：public void com.yucaihuang.service.impl.AccountServiceImpl.saveAccount()
    访问修饰符可以省略：
    void com.yucaihuang.service.impl.AccountServiceImpl.saveAccount()
    返回值可以使用通配符，表示任意返回值
    * com.yucaihuang.service.impl.AccountServiceImpl.saveAccount()
    包名可以使用通配符，表示任意包。但是有几级包就写几个*.
    * *.*.*.*.AccountServiceImpl.saveAccount()
    包名可以是使用..表示当前包及其子包
    * *..AccountServiceImpl.saveAccount()
    类名可以用*表示当前包下的所有类
    * *..*.saveAccount()
    方法名同理：
    * *..*.*()
    方法的参数可用..表示任意参数
    也可以直接写数据类型：
			基本类型直接写名称				Integer
			引用类型写包名.类名的方法 java.lang.String
    全通配写法：* *..*.*(..)				
    
实际开发中，切入点表达式的通常写法：切到业务层实现类下的所有方法
		* com.yucaihuang.service.impl.*.*(..)
```

bean.xml文件配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

<!--    配置spring的Ioc，把service对象配置进来-->
    <bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl"/>

<!--    Spring中基于xml的AOP配置步骤-->

<!--    配置Logger类-->
    <bean id="logger" class="com.yucaihuang.utils.Logger"/>

<!--    配置AOP-->
    <aop:config>
<!--        配置切面-->
        <aop:aspect id="logAdvice" ref="logger">
<!--            配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <aop:before method="printLog" pointcut="execution(* com.yucaihuang.service.impl.*.*(..))"/>
        </aop:aspect>
    </aop:config>
</beans>

```

### 四种常用类型通知

```xml
<!--            配置切入点表达式：id属性用于指定表达式的唯一标识。expression属性用于指定表达式内容
    此标签写在aop:aspect标签内部，只能当前切面使用，它还可以写在aop:aspect标签外面，所有切面可用
    必须放在aspect标签之前
-->
        <aop:pointcut id="pt1" expression="execution(* com.yucaihuang.service.impl.*.*(..))"/>
<!--        配置切面-->
        <aop:aspect id="logAdvice" ref="logger">
<!--            配置通知的类型，并且建立通知方法和切入点方法的关联-->
<!--            前置通知：在切入点方法执行之前执行-->
            <aop:before method="beforePrintLog" pointcut-ref="pt1"/>
<!--            后置通知：在切入点方法正常执行之后执行，与异常通知永远只能执行一个-->
            <aop:after-returning method="afterReturningPrintLog" pointcut-ref="pt1"/>
<!--            异常通知：在切入点方法执行产生异常之后执行，与后置通知永远只能执行一个-->
            <aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"/>
<!--            最终通知：无论切入点方法是否正常执行，它都会在其后面执行-->
            <aop:after method="afterPrintLog" pointcut-ref="pt1"/>
				</aop:aspect>
```



### Spring中的环绕通知

```markdown
* 当配置了环绕通知之后，切入点方法没有执行，而通知方法执行了。
		通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用（method.invoke()），而我们的代码中没有。
		spring框架为我们提供了一个接口，ProceedingJoinPoint。该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。该接口可以作为环绕通知的方法参数，在程序执行时，Spring框架会为我们提供该接口的实现类供我们使用。

环绕通知：它是Spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。
```

```java
/**
* 环绕通知
*
*/
public Object aroundPrintLog(ProceedingJoinPoint pjp){
  Object rtValue = null;
  //明确调用业务层方法
  try{
    Object[] args = pjp.getArgs();
    System.out.println("前置通知");
    rtValue = pjp.proceed(args);
    System.out.println("后置通知");
    return rtValue;
  }catch (Throwable t){
    System.out.println("异常通知");
    throw new RuntimeException(t);
  }finally {
    System.out.println("最终通知");
  }
}
```



### Spring基于注解的AOP配置

```java
@Component("logger")
@Aspect//表示当前类是一个切面类
public class Logger {

    @Pointcut("execution(* com.yucaihuang.service.impl.*.*(..))")
    private void pt1(){

    }
    /**
     *  前置通知
     */
    //@Before("pt1()")
    public void beforePrintLog(){
        System.out.println("前置通知日志执行");
    }
    /**
     *  后置通知
     */
    //@AfterReturning("pt1()")
    public void afterReturningPrintLog(){
        System.out.println("后置通知日志执行");
    }
    /**
     *  异常通知
     */
    //@AfterThrowing("pt1()")
    public void afterThrowingPrintLog(){
        System.out.println("异常通知日志执行");
    }
    //@After("pt1()")
    public void afterPrintLog(){
        System.out.println("最终通知日志执行");
    }
    /**
     * 环绕通知
     *
     */
    @Around("pt1()")
    public Object aroundPrintLog(ProceedingJoinPoint pjp){
        Object rtValue = null;
        //明确调用业务层方法
        try{
            Object[] args = pjp.getArgs();
            System.out.println("前置通知");
            rtValue = pjp.proceed(args);
            System.out.println("后置通知");
            return rtValue;
        }catch (Throwable t){
            System.out.println("异常通知");
            throw new RuntimeException(t);
        }finally {
            System.out.println("最终通知");
        }
    }
}
```

主配置类`SpringConfiguration.java`：

```java
@Configuration
@ComponentScan(basePackages = "com.yucaihuang")
@EnableAspectJAutoProxy
public class SpringConfiguration {
}
```



### Spring基于xml的AOP事务控制

Bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">


<!--    &lt;!&ndash;    告知Spring在创建容器时，要扫描的包&ndash;&gt;-->
<!--    <context:component-scan base-package="com.yucaihuang"/>-->
    <!--    配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
    </bean>

    <!--    配置数据源-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--        连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"/>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
        <property name="user" value="root"/>
        <property name="password" value="qazwsxedc7410"/>
    </bean>

    <bean id="accountDao" class="com.yucaihuang.dao.impl.AccountDaoImpl">
        <property name="runner" ref="runner"/>
        <property name="connectionUtils" ref="connectionUtils"/>
    </bean>

    <bean id="connectionUtils" class="com.yucaihuang.utils.ConnectionUtils">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <!--        <property name="transactionManager" ref="transactionManager"/>-->
    </bean>

    <bean id="transactionManager" class="com.yucaihuang.utils.TransactionManager">
        <property name="connectionUtils" ref="connectionUtils"/>
    </bean>


<!--    配置AOP-->
    <aop:config>
<!--        配置通用切入点表达式-->
        <aop:pointcut id="pt1" expression="execution(* com.yucaihuang.service.impl.*.*(..))"/>
        <aop:aspect id="txAdice" ref="transactionManager">
<!--            配置前置通知：开启事务-->
            <aop:before method="beginTransaction" pointcut-ref="pt1"/>
<!--            配置后置通知：提交事务-->
            <aop:after-returning method="commit" pointcut-ref="pt1"/>
<!--            配置异常通知：回滚事务-->
            <aop:after-throwing method="rollback" pointcut-ref="pt1"/>
<!--            配置最终通知：释放连接-->
            <aop:after method="release" pointcut-ref="pt1"/>

        </aop:aspect>
    </aop:config>
</beans>

```



### Spring基于注解的AOP事务控制

由于Spring框架中，最终通知先于后置通知执行，只能使用环绕通知配置事务控制。

```java
/**
 * 和事务管理相关的工具类，它包含了：开启事务，提交事务，回滚事务和释放连接
 */
@Component("transactionManager")
@Aspect
public class TransactionManager {

    @Autowired
    private ConnectionUtils connectionUtils;

    @Pointcut("execution(* com.yucaihuang.service.impl.*.*(..))")
    private void pt1(){}

    @Around("pt1()")
    public Object aroundAdvice(ProceedingJoinPoint pjp){
        Object rtValue = null;
        try {
            //1.获取参数
            Object[] args = pjp.getArgs();
            //2.开启事务
            this.beginTransaction();
            //3.执行方法
            rtValue = pjp.proceed(args);
            //4.提交事务
            this.commit();
            return rtValue;
        }catch (Throwable e){
            //5.回滚事务
            this.rollback();
            throw new RuntimeException(e);
        }finally {
            //6.释放资源
            this.release();
        }
    }
}
```

