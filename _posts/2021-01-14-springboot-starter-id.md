---
title: 创建自定义Springboot starter
author: Kol Huang
date: 2021-01-14 10:11:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
---





以redis为例，创建一个自定义的Springboot starter。

首先创建一个工程，命名为redis-spring-boot-starter。

添加Jar包依赖：Redisson提供了在Java中操作Redis的功能，并且基于Redis的特性封装了很多可直接使用的场景，比如分布式锁。

```xml
<dependency>
  <groupId>org.redisson</groupId>
  <artifactId>redisson-all</artifactId>
  <version>3.14.1</version>
</dependency>
```

定义属性类xxxProperties。@ConfigurationProperties注解把当前类中的属性和配置文件(application.properties)中的配置进行绑定，并且前缀是gp.redisson。

```java
package com.hyc.redisspringbootstarter.config;
import org.springframework.boot.context.properties.ConfigurationProperties;
/**
 * @author kol Huang
 * @date 2021/1/14
 */

@ConfigurationProperties(prefix = "gp.redisson")
public class RedissonProperties {

    private String host = "localhost";
    private String password;
    private int port = 6379;
    private int timeout;
    private boolean ssl;
  
    public String getHost() {
        return host;
    }

    public void setHost(String host) {
        this.host = host;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public int getPort() {
        return port;
    }

    public void setPort(int port) {
        this.port = port;
    }

    public int getTimeout() {
        return timeout;
    }

    public void setTimeout(int timeout) {
        this.timeout = timeout;
    }

    public boolean isSsl() {
        return ssl;
    }

    public void setSsl(boolean ssl) {
        this.ssl = ssl;
    }
}

```



定义需要自动装配的配置类：把RedissonCilent装配到IoC容器中。

```java
package com.hyc.redisspringbootstarter.config;

import org.redisson.Redisson;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;
import org.redisson.config.SingleServerConfig;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.util.StringUtils;

/**
 * @author kol Huang
 * @date 2021/1/14
 */
@Configuration
@ConditionalOnClass(Redisson.class)
@EnableConfigurationProperties(RedissonProperties.class)
public class RedissonAutoConfiguration {

    @Bean
    RedissonClient redissonClient(RedissonProperties redissonProperties){
        Config config = new Config();
        String prefix = "redis://";
        if(redissonProperties.isSsl()){
            prefix = "rediss://";
        }

        SingleServerConfig singleServerConfig = config.useSingleServer()
                .setAddress(prefix + redissonProperties.getHost() + ":" + redissonProperties.getPort())
                .setConnectTimeout(redissonProperties.getTimeout());

        if(!StringUtils.isEmpty(redissonProperties.getPassword())){
            singleServerConfig.setPassword(redissonProperties.getPassword());
        }

        return Redisson.create(config);
    }
}

```



在resources目录下创建META/INF/spring.factories文件，使得Spring Boot程序可以扫描到该文件完成自动装配。

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.hyc.redisspringbootstarter.config.RedissonAutoConfiguration
```



在命令行中输入`mvn clean install`将此starter项目打包成jar包放入本地仓库。

打开另外一个springboot项目，引入依赖：

```xml
<dependency>
  <groupId>com.hyc</groupId>
  <artifactId>redis-spring-boot-starter</artifactId>
  <version>0.0.1-SNAPSHOT</version>
</dependency>
```

就可以在application.properties中配置redis了。



</br>

</br>

参考自《Spring Cloud Alibaba》微服务原理与实践