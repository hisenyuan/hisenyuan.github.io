---
title: Ubuntu16.04使用阿里云镜像安装Mongodb &　配置
keywords: []
tags: []
date: 2017-02-19 22:36:25
categories:
---

一、使用阿里云镜像安装Mongodb
---
1 > 添加 MongoDB 公共GPG钥匙
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
```
2 > 创建列表文件

这里把官网repo.mongodb.org

换成了mirrors.aliyun.com
```
echo "deb http://mirrors.aliyun.com/mongodb/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
```
<!--more-->
3 > 重新加载本地包数据库
```
sudo apt-get update
```
4 > 安装MongoDB
```
sudo apt-get install -y mongodb-org
```
5 > 启动MongoDB
```
sudo service mongod start
```
6 > 打开MongoDB客户端
```
sudo mongo
```
7 > 关闭MongoDB
```
sudo service mongod stop
```

展示一下
```
hisen@hisen-server:~$ sudo service mongod start
hisen@hisen-server:~$ mongo
MongoDB shell version: 3.2.12
connecting to: test
> 1 + 1
2
> 
```
安装成功

MongoDB默认的数据文件和日志文件分别存储在下面的位置
 
数据文件：/var/lib/mongodb 

日志文件：/var/log/mongodb 

你可以修改/etc/mongod.conf 文件来改变相应的存储位置。

如果你想改变运行MongoDB的用户

你必须把 /var/lib/mongodb 

和 /var/log/mongodb 2个目录的访问权限付给该用户

二、配置MongoDB
---
1允许远程访问

绑定ip
```
$ sudo vim /etc/mongod.conf
```
打开配置文件，添加需要增加的

不建议采用注释掉  bindIP 的方案，非常容易受到攻击
```
# network interfaces  
net:  
  port: 27017  
  bindIp: 0.0.0.0
```
接受所有ip

重启
```
$ sudo service mongod restart
```

2.配置防火墙 （不配置也行）
Ubuntu16.04 桌面版默认没有安装好 ipTable，用如下命令安装
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install iptables-persistent
```
安装过程中，弹窗选择YES

安装完成后：
```
sudo iptables -A INPUT -p tcp --dport 27017 -j ACCEPT
```