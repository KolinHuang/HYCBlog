---
title: centOS误删系统自带python
author: Kol Huang
date: 2020-08-26 21:14:00 +0800
categories: [Blogging, web]
tags: [web]
comments: true
math: true
---



##  解决centOS误删系统自带python后，yum不可用的问题

前几天手贱，为了装python3把系统自带的2.7给删了，今天才发现yum用不了了...

报如下错误：

![centos_yum报错](/HYCBlog/assets/img/web/centos_yum报错.png)

上网查了发现，是因为yum依赖了系统自带的python，于是我就去找了2.7.5的python包，安装了之后还是报上面的错...

试着在2.7.5下import yum，发现不存在这个模块。所以只能去下载[源码包](http://vault.centos.org/7.3.1611/os/x86_64/Packages/)安装了。

### 第一步

首先卸载python:

```sh
rpm -qa|grep python|xargs rpm -e --allmatches --nodeps
whereis python|xargs rm -fr
```

再卸载yum:

```sh
rpm -qa|grep yum|xargs rpm -e --allmatches --nodeps
rm -rf /etc/yum.repos.d/*
whereis yum|xargs rm -fr
```



### 第二步

在`/usr/local/src/`目录下新建两个子目录`python`和`yum`

去[网址](http://vault.centos.org/7.3.1611/os/x86_64/Packages/)里下载以下安装包放入`python`子目录：

```markdown
python-libs-2.7.5-48.el7.x86_64.rpm，被python依赖
python-2.7.5-48.el7.x86_64.rpm
python-iniparse-0.4-9.el7.noarch.rpm， 被yum依赖
python-pycurl-7.19.0-19.el7.x86_64.rpm, 被python-urlgrabber依赖
python-urlgrabber-3.10-8.el7.noarch.rpm ， 被yum依赖
rpm-python-4.11.3-21.el7.x86_64.rpm ， 被yum依赖
```

再下载以下安装包放入`yum`目录：

```markdown
yum-3.4.3-150.el7.centos.noarch.rpm, 就是它依赖了上面的python库
yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
yum-plugin-fastestmirror-1.1.31-40.el7.noarch.rpm
```



### 第三步

接下来分别进入`python`和`yum`子目录执行：

```sh
rpm -ivh xxx.rpm
```

注意，安装yum时需要同时安装三个包，这样可以避免相互依赖导致安装出错。

```sh
rpm -ivh yum-3.4.3-150.el7.centos.noarch.rpm yum-metadata-parser-1.1.4-10.el7.x86_64.rpm yum-plugin-fastestmirror-1.1.31-40.el7.noarch.rpm
```



至此，测试`python`和`yum`均可正常执行！



参考链接：

[Centos7卸载Python2.7之后恢复yum](https://www.jianshu.com/p/89df82a5d74b)





淦！！！防火墙也挂了，我🤮了啊，以后不随便在服务器改python版本了😭

![centos_防火墙报错](/HYCBlog/assets/img/web/centos_防火墙报错.png)

