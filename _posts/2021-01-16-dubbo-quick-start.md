---
title: Dubbo：简单使用
author: Kol Huang
date: 2021-01-16 14:08:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
---

 



简单记录Dubbo的入门使用。dubbo的主体架构如下图所示：

![image-20210116134900615](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210116134900615.png)

创建两个普通Maven工程，user-service和order-service。其中，user-service是是服务提供者，order-service是服务订阅者，注册中心和监控中心均不设置。

### 第一步

在user-service服务中创建两个模块，分别是user-api和user-provider。在user-api中定义服务接口，供订阅者远程调用。

```java
public interface IUserService{
  	String getName(String id);
}
```

然后在终端执行命令mvn clean install将user-api打包成jar包放入本地仓库。也可以通过命令mvn deploy将其发布到远程私服。

在user-provider中编写接口的实现，首先需要在pom.xml中引入依赖：

```xml
<dependency>
  <groupId>com.hyc</groupId>
  <artifactId>user-api</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>


<dependency>
  <groupId>org.apache.dubbo</groupId>
  <artifactId>dubbo</artifactId>
  <version>2.7.8</version>
</dependency>
```

然后实现接口：

```java
/**
 * @author kol Huang
 * @date 2021/1/16
 */
public class UserServiceImpl implements IUserService {

    public String getNameById(String s) {
        System.out.println("receive data: " + s);
        return "hyc";
    }
}
```

配置dubbo：在resources目录下创建文件META-INF/spring/user-provider.xml配置文件，在配置文件中配置以下信息：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="user-service"  />

    <!-- 使用multicast广播注册中心暴露服务地址 -->
    <dubbo:registry address="N/A" />

    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />

    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="IUserService" ref="userService" />

    <!-- 和本地bean一样实现服务 -->
    <bean id="userService" class="UserServiceImpl" />
</beans>
```

其中：

* `dubbo:application`用来描述提供方的应用信息，比如应用名称、维护人、版本等。必填项。
* `dubbo:registry`配置注册中心的地址，如果不需要配置中心，就设置为N/A。Dubbo支持多种注册中心，如ZooKeeper, Nacos等。
* `dubbo:protocol`配置服务提供者的协议信息，默认为dubbo协议。
* `dubbo:service`描述需要发布的服务接口。

最后读取dubbo配置，启动dubbo服务：

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.IOException;

/**
 * @author kol Huang
 * @date 2021/1/16
 */
public class DubboMain {

    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("classpath*:/META-INF/spring/user-provider.xml");
        context.start();
        System.in.read();
    }
}
```



控制台输出接口的发布信息：

```shell
dubbo://192.168.199.169:20880/IUserService?anyhost=true&application=user-service&bind.ip=192.168.199.169&bind.port=20880&deprecated=false&dubbo=2.0.2&dynamic=true&generic=false&interface=IUserService&methods=getNameById&pid=60750&release=2.7.8&revision=1.0-SNAPSHOT&side=provider&timestamp=1610766085178, dubbo version: 2.7.8, current host: 192.168.199.169
```

可以看出，接口的调用地址为：dubbo://192.168.199.169:20880/IUserService。`?`后面的都是dubbo的配置信息。



### 第二步

在order-provider服务中远程调用dubbo发布的接口。

首先引入依赖：

```xml
<dependency>
  <groupId>com.hyc</groupId>
  <artifactId>user-api</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>


<dependency>
  <groupId>org.apache.dubbo</groupId>
  <artifactId>dubbo</artifactId>
  <version>2.7.8</version>
</dependency>
```

然后配置Consumer信息，在resources目录下编写文件META-INF/spring/Consumer.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="consumer-of-user-service"  />

    <dubbo:registry address="N/A" />

    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="userService" interface="IUserService"
    url="dubbo://192.168.199.169:20880/IUserService"/>
</beans>
```

* `dubbo:reference`生成远程服务代理，interface属性指定提供服务的接口信息，url属性指定远程服务的调用地址。



执行远程调用：

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.IOException;

/**
 * @author kol Huang
 * @date 2021/1/16
 */
public class MainDubbo {

    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("classpath*:/META-INF/spring/consumer.xml");
        IUserService userService = (IUserService) context.getBean("userService");
        System.out.println(userService.getNameById("1001"));//输出“hyc”
    }
}

```





参考Dubbo官方文档及SpringCloud Alibaba

