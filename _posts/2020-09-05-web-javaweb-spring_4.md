---
title: Spring：IOC（控制反转）
author: Kol Huang
date: 2020-09-05 21:41:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---





IoC把创建对象的权利交给框架。它包括依赖注入（Dependency Injection）和依赖查找（Dependency Lookup）。

作用：<span style="color:red">削减计算机程序的耦合</span>（解除代码中的依赖关系）



## spring基于xml的IOC环境搭建和入门

在pom.xml中导入坐标：

```xml
				<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.7.RELEASE</version>
        </dependency>
```

在resource资源目录下创建bean.xml配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    把对象的创建交给spring来管理-->
    <bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl"/>
    <bean id="accountDao" class="com.yucaihuang.dao.impl.AccountDaoImpl"/>
</beans>
```

在类中根据id获取对象：

```java
/**
     * 获取Spring的IoC核心容器，并根据id获取对象
     * @param args
     */
    public static void main(String[] args) {

        //1 获取核心容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2 根据id获取Bean对象
        IAccountService as = (IAccountService) ac.getBean("accountService");
        IAccountDao accountDao = ac.getBean("accountDao",IAccountDao.class);

        System.out.println(as);
        System.out.println(accountDao);
    }
```



## ApplicationContext的三个常用实现类

```markdown
ClassPathXmlApplicationContext:
	它可以加载类路径下的配置文件，要求配置文件必须在类路径下。不在的话，加载不了。（常用）
FileSystemApplicationContext：
	它可以加载磁盘任意路径下的配置文件（必须有访问权限）
AnnotationConfigApplicationContext
	是用于读取注解创建容器的

```



## BeanFactory和ApplicationContext的区别

```markdown
ApplicationContext:	（单例对象适用）（常用）
	它在构建核心容器时，创建对象采取的策略是采用立即加载的方式。也就是说，只要一读取完配置文件，马上就创建配置文件中配置的对象。
BeanFactory：	（多例对象适用）
	它在构建核心容器时，创建对象采取的策略是采用延迟加载的方式。也就是说，什么时候根据id获取对象了，什么时候才真正的创建对象。
```



## Spring中Bean的细节



### 创建Bean对象的三种方式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    把对象的创建交给spring来管理-->
<!--    Spring对bean的管理细节
        1、创建bean对象的三种方式
        2、bean对象的作用范围
        3、bean对象的生命周期
-->

<!--    创建Bean的三种方式-->
<!--    第一种方式：使用默认构造函数创建
                在Spring的配置文件中使用bean标签，配以id和class属性之后，且没有其他属性和标签时，
                采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则对象无法创建。-->
				<bean id="accountService" class="com.yucaihuang.service.IAccountService"/>



<!--    第二种方式：使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入Spring容器）
				factory-bean属性用于指定要使用的工厂类，factory-method用于指定工厂中用于创建我们所需对象的方法-->
				<bean id="InstanceFactory" class="com.yucaihuang.factory.InstanceFactory"/>
				<bean id="accountService" factory-bean="InstanceFactory" factory-method="getAccountService"/>


<!--    第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入Spring容器）-->
    <bean id="accountService" class="com.yucaihuang.factory.StaticFactory" factory-method="getAccountService"/>
</beans>
```

```java
public class InstanceFactory {
    public IAccountService getAccountService(){
        return new AccountServiceImpl();
    }
}
```

```java
public class StaticFactory {
    public static IAccountService getAccountService(){
        return new AccountServiceImpl();
    }
}
```



### 作用范围

```xml
<!--    bean的作用范围调整
            bean标签的scope属性：
            作用：用于指定bean的作用范围
            取值：常用前两个属性 
                singleton：单例的（默认值）
                prototype：多例的
                request：作用于web应用的请求范围
                session：作用于web应用的会话范围
                global-session：作用于集群环境的会话范围（全局会话范围），当不是集群环境时，它就是session
-->
    <bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl" scope="prototype"/>
```



### 生命周期

```markdown
*	单例对象
		出生：当容器创建时对象出生
		活着：只要容器存在，对象就一直活着
		死亡：容器销毁，对象消亡
		总结：单例对象的生命周期和容器相同
*	多例对象
		出生：当使用对象时，Spring框架为我们创建
		活着：对象只要在使用过程中就一直活着
		死亡：当对象长时间不用，且没有其他对象引用它时，由Java垃圾收集器回收。
```



## Spring的依赖注入

IOC的作用时为了降低程序间的耦合，而耦合指的是类之间的依赖关系，依赖关系的管理都交给Spring来维护，在当前类需要用到其他类的对象时，由Spring为我们提供，我们只需要在配置文件中说明。

依赖关系的维护就称为依赖注入：

```markdown
能注入的数据有三类：
1. 基本类型和String
2. 其他Bean类型（在配置文件中或者注解配置过的bean）
3. 复杂类型/集合类型
```

注入的方式有三种：

1. **使用构造函数提供（不常用）**
   	

   ```markdown
   使用的标签：constructor-arg
   标签中的属性：
    type: 用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
     index: 用于指定要注入的数据在构造函数的参数的索引位置，从0开始。
     （常用）name: 用于指定给构造函数中指定名称的参数赋值
     -----------------------以上三个用于指定给构造函数中的哪个参数赋值--------------------
     value: 用于提供基本类型和String类型的数据
     ref: 用于指定其他的bean类型数据。它指的就是在Spring的IoC核心容器中出现过的bean对象
   ```

   ```xml
   <bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl">
           <constructor-arg name="age" value="18"/>
           <constructor-arg name="birthday" ref="now"/>
           <constructor-arg name="name" value="张塞"/>
   </bean>
   
   <bean id="now" class="java.util.Date"/>
   ```

   * 优点：在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。
   * 缺点：改变了bean对象的实例化方法，使我们在创建对象时，如果用不到这些数据，也必须提供。

   

2. **使用set方法提供**

   ```markdown
   使用的标签：property
   标签中的属性：
   	name：用于指定注入时，所调用的set方法名称。
   	value：用于提供基本类型和String类型的数据。
   	ref：用于指定其他的bean类型数据。
   ```

   ```xml
   <bean id="now" class="java.util.Date"/>
   
   <bean id="accountService2" class="com.yucaihuang.service.impl.AccountServiceImpl2">
     <property name="age" value="22"/>
     <property name="name" value="hehe"/>
     <property name="birthday" ref="now"/>
   </bean>
   
   <!--复杂类型的注入/集合类型的注入
   		用于给list结构集合注入的标签：list, array, set
   		用于给Map集合注入的标签：map. props
   		结构相同，标签可以互换
   -->
   <bean id="accountService3" class="com.yucaihuang.service.impl.AccountServiceImpl3">
     <property name="myStrs">
       <array>
         <value>AAA</value>
         <value>BBB</value>
         <value>CCC</value>
       </array>
     </property>
     
     <property name="myList">
       <List>
         <value>AAA</value>
         <value>BBB</value>
         <value>CCC</value>
       </List>
       
     </property>
       <property name="mySet">
       <Set>
         <value>AAA</value>
         <value>BBB</value>
         <value>CCC</value>
       </Set>
         
     </property>
       <property name="myMap">
       <Map>
         <entry key="testA" value="AAA"/>
         <entry key="testB">
           <value>BBB</value>
         </entry>
       </List>
     </property>
     
       <property name="myProps">
       <props>
   			<prop key="testC">CCC</prop>
       </props>
     </property>
     
     
   ```

   * 优点：创建对象时，没有明确的限制，可以直接使用默认构造函数
   * 缺点：如果有某个成员必须有值，则获取对象时有可能set方法没有执行。

3. **使用注解提供**

    

   **注解类型**：

   1. 用于创建对象的注解：类似于编写一个<bean>标签的作用:

      ```markdown
      @Component:
      		作用：用于把当前类对象存入Spring容器中国呢
      		属性：value：用于指定bean的id。但缺省时，默认值时当前类名，且首字母改小写。
      
      @Controller：一般用于表现层
      @Service：一般用于业务层
      @Repository：一般用于持久层
      以上三个注解的作用和属性与Component一模一样。是Spring框架为我们提供明确的三层使用的注解，使我们的三层对象更加清晰。
      ```

   2. 用于注入数据的注解：类似于在<bean>标签中编写<properties>的作用：

      ```markdown
      @Autowired:
      	作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功。如果IOC容器中没有任何bean的类型和要注入的变量类型匹配，则会报错。如果容器中出现了多个bean对象与要注入的变量类型匹配，那么会比较变量名和bean对象名是否匹配，否则报错。
      	出现位置：可以是变量上，也可以是方法上。
        细节：在使用注解注入时，set方法就不是必须的了。
      
      @Qualifier:
      	作用：在按照类中注入的基础之上，再按照名称注入。它在给类成员注入时不能单独使用。但是在给方法参数注入时可以单独使用。
      	属性：value：用于指定注入bean的id。
      
      @Resource:
      	作用：直接按照bean的id注入。可以独立使用
      	属性：name：用于指定bean的id.
      ```

      以上三个注解都只能注入其他bean类型的数据，而基本类型和String类型无法使用。

      ```markdown
      @Value
      	作用：用于注入基本类型和String类型的数据
      	属性：value：用于指定数据的值。它可以使用Spring中的SpEL，也就是Spring的EL表达式。
      																							SpEL的写法：${表达式}
      ```

      

      另外，<span style="color:red">集合类型的注入只能通过xml实现。</span>

      ​	

   3. 用于改变作用范围的注解：类似于在<bean>标签中编写scope属性的作用：

      ```markdown
      @Scope
      	作用：用于指定bean的作用范围
      	属性：value：指定范围的取值。常用取值：singleton prototype。默认单例。
      ```

   4. 和生命周期相关的注解：类似于在<bean>标签中编写init-method和destory-method的作用。

      ```markdown
      @PreDestory：用于指定销毁方法
      @PostConstruct：用于指定初始化方法
      ```



## IOC案例



### 案例准备

定义账户实体类：

```java
/**
 * 账户的实体类
 */
public class Account implements Serializable {
    private Integer id;
    private String name;
    private Float money;

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }
}

```



定义账户持久层接口：

```java
/**
 * 账户业务层接口
 */
public interface IAccountService {

    /**
     * 查询所有
     * @return
     */
    List<Account> findAllAccount();

    /**
     * 查询一个
     * @param id
     * @return
     */
    Account findAccountById(Integer id);

    void saveAccount(Account account);

    void updateAccount(Account account);

    void deleteAccount(Integer id);
}
```

定义账户持久层接口实现类：

```java
public class AccountDaoImpl implements IAccountDao {

  private QueryRunner runner;

  public void setRunner(QueryRunner runner) {
    this.runner = runner;
  }

  public List<Account> findAllAccount() {
    try {
      return runner.query("select * from account",new BeanListHandler<Account>(Account.class));
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public Account findAccountById(Integer id) {
    try {
      return runner.query("select * from account where id = ?",new BeanHandler<Account>(Account.class),id);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void saveAccount(Account account) {
    try {
      runner.update("insert into account(name,money)values(?,?)",account.getName(),account.getMoney());
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void updateAccount(Account account) {
    try {
      runner.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }

  public void deleteAccount(Integer id) {
    try {
      runner.update("delete from account where id=?",id);
    }catch (Exception e){
      throw new RuntimeException(e);
    }
  }
}

```

定义账户业务层接口：

```java
/**
 * 账户业务层接口
 */
public interface IAccountService {

    /**
     * 查询所有
     * @return
     */
    List<Account> findAllAccount();

    /**
     * 查询一个
     * @param id
     * @return
     */
    Account findAccountById(Integer id);

    void saveAccount(Account account);

    void updateAccount(Account account);

    void deleteAccount(Integer id);
}
```

定义账户业务层实现类：

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
}
```



### 编写Spring的IoC配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    配置service-->
    <bean id="accountService" class="com.yucaihuang.service.impl.AccountServiceImpl">
<!--        注入dao-->
        <property name="accountDao" ref="accountDao"/>
    </bean>

<!--    配置dao对象-->
    <bean id="accountDao" class="com.yucaihuang.dao.impl.AccountDaoImpl">
<!--        注入QueryRunner-->
        <property name="runner" ref="runner"/>
    </bean>

<!--    配置QueryRunner-->
  <!--为了在并发情况下，保证线程安全，采用多例方式创建runner，这样可以使多个操作不会互相影响，在每个线程中都创建一个runner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
<!--        注入数据源-->
      <!--使用构造器注入-->
        <constructor-arg name="ds" ref="dataSource"/>
    </bean>

<!--    配置数据源-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!--        连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"/>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
        <property name="user" value="root"/>
        <property name="password" value="qazwsxedc7410"/>
    </bean>
</beans>

```

### 测试基于xml的IoC案例

```java
/**
 * 使用Junit单元测试进行操作
 */
public class AccountServiceTest {

    @Test
    public void testFindAll(){
        //获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //根据id从容器中取出bean对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        //调用bean对象的方法实现功能
        List<Account> allAccount = as.findAllAccount();
        for (Account account : allAccount) {
            System.out.println(account);
        }
    }
    @Test
    public void testFindOne(){

    }
    @Test
    public void testSave(){

    }
    @Test
    public void testUpdate(){

    }
    @Test
    public void testDetele(){

    }
}

```



## Spring的新注解

用于替代bean.xml配置文件

```markdown
1. @configuration
		作用：指定当前类是一个配置类
		细节：当配置类作为AnnotationConfigApplicationContext对象创建的参数时，该注解可以不写。
2. @ComponentScan
		作用：用于通过注解指定Spring在创建容器时要扫描的包。
		属性：value：它和basePackages的作用是一样的，都是用于指定创建容器时要扫描的包。
3. @Bean
		作用：用于把当前方法的返回值作为bean对象存入Spring的ioc容器中。
		属性：name：用于指定bean的id。当不写时，默认值是当前方法的名称。
		细节：当我们使用注解配置方法时。如果方法有参数，Spring框架回去容器中查找有没有可用的bean对象。
				 查找的方式和@Autowired注解的作用是一样的。
4. @Scope
		设置单例或多例
5. @import
		作用：用于导入其他的配置类
		属性：value：用于指定其他配置类的字节码，当我们使用import的注解之后，有Import注解的类就是父配置类，而导入的都是子配置类。
		
6. @PropertySource
		
```

<span style="color:red">当要配置的类存在于jar包中时，利用xml方法配置较为方便；当要配置的类是自己写的时，利用注解方式配置较为方便。</span>



编写主配置类：

```java
/**
 * 该类是一个配置类，它的作用和bean.xml是一样的
 *
 */

//@Configuration
@ComponentScan(basePackages = "com.yucaihuang")
@Import(JdbcConfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {


}
```

编写子数据类用于连接数据库：

```java
/**
 * 和Spring连接数据库相关的配置类
 */
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
    /**
     * 用于创建一个QueryRunner对象
     * @param dataSource
     * @return
     */
    @Bean(name = "runner")
    @Scope(value = "prototype")
    public QueryRunner createQueryRunner(@Qualifier("dataSource2") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name = "dataSource")
    public DataSource createDataSource(){
        try{
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }

    }
    @Bean(name = "dataSource2")
    public DataSource createDataSource2(){
        try{
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```



测试：

```java
@Test
    public void testFindAll(){
        //获取容器
//        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        ApplicationContext ac = new AnnotationConfigApplicationContext(SpringConfiguration.class);
        //根据id从容器中取出bean对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        //调用bean对象的方法实现功能
        List<Account> allAccount = as.findAllAccount();
        for (Account account : allAccount) {
            System.out.println(account);
        }
    }
```



