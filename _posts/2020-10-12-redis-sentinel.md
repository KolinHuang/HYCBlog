---
title: 用Docker搭建一套Redis Sentinel集群
author: Kol Huang
date: 2020-10-12 20:51:00 +0800
categories: [Blogging, web]
tags: [redis]
comments: true
math: true
---



集群配置：一个主节点(master)、两个从节点(slave)、三个哨兵(sentinel)。

以下步骤省略：

1. 安装docker；
2. 拉取redis镜像；

## 1.获取并修改redis配置文件

```shell
$ wget http://download.redis.io/redis-stable/redis.conf	#官方提供的配置文件样例
```

拷贝这个文件3份，一份为master配置文件，其他两份为slave配置文件。

### 1.1配置master文件

```shell
# 注释这一行，表示Redis可以接受任意ip的连接
# bind 127.0.0.1 

# 关闭保护模式
protected-mode no 

# 让redis服务后台运行
daemonize yes 

# 设定密码(可选，如果这里开启了密码要求，slave的配置里就要加这个密码. 只是练习配置，就不使用密码认证了)
# requirepass masterpassword 

# 配置日志路径，为了便于排查问题，指定redis的日志文件目录
logfile "/var/log/redis/redis.log"
```



### 1.2配置slave文件

```shell
# 注释这一行，表示Redis可以接受任意ip的连接
# bind 127.0.0.1 

# 关闭保护模式
protected-mode no 

# 让redis服务后台运行
daemonize yes 

#不用修改端口号


# 设定密码(可选，如果这里开启了密码要求，slave的配置里就要加这个密码)
requirepass masterpassword 

# 设定主库的密码，用于认证，如果主库开启了requirepass选项这里就必须填相应的密码
masterauth <master-password>

# 设定master的IP和端口号，redis配置文件中的默认端口号是6379
# 低版本的redis这里会是slaveof，意思是一样的，因为slave是比较敏感的词汇，所以在redis后面的版本中不在使用slave的概念，取而代之的是replica
# 将127.0.0.1做为主，其余两台机器做从。ip和端口号按照机器和配置做相应修改。
replicaof 127.0.0.1 6379

# 配置日志路径，为了便于排查问题，指定redis的日志文件目录
logfile "/var/log/redis/redis.log"
```



## 2.启动容器

```shell
# docker run -it --name redis-1 -v /root/redis-master.conf:/usr/local/etc/redis/redis.conf -d -p 6379:6379 redis /bin/bash
073d5005b562f1ddff2a32efe1de9bf6cbfecf71fa0dc9023170349e334b8354
root@izbp1g4zldn8zzaga4gpidz: ~ 20:45:31
# docker run -it --name redis-2 -v /root/redis-slave01.conf:/usr/local/etc/redis/redis.conf -d -p 6380:6379 redis /bin/bash
44eab642d21cf37abd8f5b749b6e8cadd82cd3abe77424293c5aea49afbd0f04
root@izbp1g4zldn8zzaga4gpidz: ~ 20:46:15
# docker run -it --name redis-3 -v /root/redis-slave02.conf:/usr/local/etc/redis/redis.conf -d -p 6381:6379 redis /bin/bash
114439ba26fab3ea9b962ac7129227b2a4efeb0fcc8d66ba5b74f87d60185aa3
root@izbp1g4zldn8zzaga4gpidz: ~ 20:46:27
# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                              NAMES
114439ba26fa        redis               "docker-entrypoint.s…"   4 seconds ago        Up 3 seconds        6379/tcp, 0.0.0.0:6381->6381/tcp   redis-3
44eab642d21c        redis               "docker-entrypoint.s…"   17 seconds ago       Up 16 seconds       6379/tcp, 0.0.0.0:6380->6380/tcp   redis-2
073d5005b562        redis               "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6379->6379/tcp             redis-
```



### 2.1进入容器启动服务

进入master容器：

```shell
# docker exec -it redis-1 bash
root@073d5005b562:/data# 
root@073d5005b562:/data# 
root@073d5005b562:/data# mkdir /var/log/redis/
root@073d5005b562:/data# touch /var/log/redis/redis.log
root@073d5005b562:/data# redis-server /usr/local/etc/redis/redis.conf
root@073d5005b562:/data# redis-cli
127.0.0.1:6379> info
# Server
redis_version:6.0.8
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:75cef67090587c6
redis_mode:standalone
os:Linux 3.10.0-514.26.2.el7.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
...
# Replication
role:master
connected_slaves:0
...
```

进入slave容器：

```shell
root@izbp1g4zldn8zzaga4gpidz: ~ 21:43:17
# docker exec -it redis-2 bash
root@c0cb4aba85f0:/data# mkdir /var/log/redis/
root@c0cb4aba85f0:/data# touch /var/log/redis/redis.log
root@c0cb4aba85f0:/data# redis-server /usr/local/etc/redis/redis.conf
root@c0cb4aba85f0:/data# redis-cli
127.0.0.1:6379> info
# Server
redis_version:6.0.8
redis_git_sha1:00000000


# Replication
role:slave
master_host:127.0.0.1
master_port:6379

# Keyspace
127.0.0.1:6379> 
```

出现问题：

`master_link_status:down`，没有连接上master，日志如下：

```shell
Connecting to MASTER 127.0.0.1:6379
15:S 12 Oct 2020 13:54:10.739 * MASTER <-> REPLICA sync started
15:S 12 Oct 2020 13:54:10.739 * Non blocking connect for SYNC fired the event.
15:S 12 Oct 2020 13:54:10.739 * Master replied to PING, replication can continue...
15:S 12 Oct 2020 13:54:10.739 * Partial resynchronization not possible (no cached master)
15:S 12 Oct 2020 13:54:10.739 * Master is currently unable to PSYNC but should be in the future: -NOMASTERLINK Can't SYNC while not connected with my master
```

