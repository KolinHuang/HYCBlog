---
title: URL包含中文，请求资源404
author: Kolin Huang
date: 2020-08-24 15:40:00 +0800
categories: [Blogging, web]
tags: [web]
comments: true
math: true
---



 

在本地时，项目路径内，中文解析均正确。但是放到服务器上后，http请求解析出现乱码。如下：

![web_请求url有中文](/HYCBlog/assets/img/web/web_请求url有中文.png)

当url路径包含中文的时候，浏览器会自动采用UTF-8对路径进行编码，而服务器（本例中是tomcat，不同服务器的实际可能有差异，但原理差不多）默认是采用ISO-8859-1来对url路径进行解码，此时往往会出现404，如以上例子所述。

所以需要在服务端的配置文件中指定编码：

1. 在tomcat中配置，server.xml设置URIEncoding="UTF-8"

   ```xml
   <Connector connectionTimeout="20000" port="8080" 
                   protocol="HTTP/1.1" redirectPort="8443" 
                   useBodyEncodingForURI="true" URIEncoding="UTF-8"/>
   ```

2. 在weblogic中配置，web logic.xml文件中指定UTF-8

   ```xml
   <input-charset> 
           <java-charset-name>UTF-8</java-charset-name> 
   </input-charset>
   ```



## 顺便解决了tomcat启动过慢的问题



在Tomcat的bin目录下找到catalina.sh中，添加

```sh
JAVA_OPTS="$JAVA_OPTS -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Djava.security.egd=file:/dev/urandom"
```

![web_tomcat启动过慢](/HYCBlog/assets/img/web/web_tomcat启动过慢.png)

参考链接：

[在URL中传递中文的解决方式](https://blog.csdn.net/ThinkingLink/article/details/45695755?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

[【WEB】url路径包含中文和表单get请求包含中文](https://blog.csdn.net/reliveIT/article/details/44927129)

[轻松解决Tomcat启动慢的问题，只需一行代码](https://blog.csdn.net/qing_gee/article/details/86705890)

