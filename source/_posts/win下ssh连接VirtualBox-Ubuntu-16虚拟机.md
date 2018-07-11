---
title: win下ssh连接VirtualBox Ubuntu 16虚拟机
keywords: [win下ssh连接VirtualBox Ubuntu 16虚拟机,Ubuntu]
tags: [linux,xshell连接Ubuntu虚拟机]
date: 2017-02-18 15:34:49
categories: linux
---
安装完虚拟机之后想在windows下用xshell链接Ubuntu虚拟机

这种配置下，虚拟机能上网，又能跟win连接，感觉很完美

VirtualBox的端口转发很不错，可以转发tomcat什么的


准备工作
---
1.给Ubuntu安装openssh-server
```
sudo apt-get install openssh-server
```
<!--more-->
2.查看虚拟机ip 我的是：10.0.2.15(看上面那段)
```
hisen@hisen-VirtualBox:/$ ifconfig -a
enp0s3    Link encap:以太网  硬件地址 08:00:27:45:f3:35  
          inet 地址:10.0.2.15  广播:10.0.2.255  掩码:255.255.255.0
          inet6 地址: fe80::ed57:82d2:60cb:7f96/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1
          接收数据包:169175 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:34436 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1000 
          接收字节:210738565 (210.7 MB)  发送字节:3633455 (3.6 MB)

lo        Link encap:本地环回  
          inet 地址:127.0.0.1  掩码:255.0.0.0
          inet6 地址: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  跃点数:1
          接收数据包:1718 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:1718 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1 
          接收字节:234769 (234.7 KB)  发送字节:234769 (234.7 KB)
```
VirtualBox设置端口转发
---
1. 在VirtualBox启动页面，右键Ubuntu --->设置
2. 网络 ---> 连接方式 ---> 网络地址转换(NAT)
3. 高级 ---> 端口转发 ---> 点击添加按钮

| 名称 | 协议 | 主机IP | 主机端口 | 子系统IP | 子系统端口 |
|:-----|:-----|:-----|:-----|:-----|:-----|
| ssh | TCP | 127.0.0.1 | 2222 | 10.0.2.15 | 22 |

子系统ip写你的虚拟机ip即可

xshell链接Ubuntu虚拟机
---
在xshell链接Ubuntu虚拟机的时候

ip写上麦的主机ip：127.0.0.1

端口写上面的主机端口：2222

然后上面配置的端口转发就可以转发到虚拟机上，顺利连接！！！
