---
title: Ubuntu 16 LTS 安装redis -  apt-get install redis-server
keywords: [redis]
tags: [redis]
date: 2017-02-23 18:26:11
categories: linux
---

配置文件的路径： /etc/redis/redis.conf

redis服务路径: /etc/init.d/redis-server

默认是开机启动

<!--more-->
```
#安装
hisen@hisen-server:/$ sudo apt-get install redis-server
#打开服务
hisen@hisen-server:/$ service redis start
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to start 'redis-server.service'.
Authenticating as: hisen,,, (hisen)
Password: 
==== AUTHENTICATION COMPLETE ===
#打开客户端
hisen@hisen-server:/$ redis-cli
#操作数据库
127.0.0.1:6379> set hisen hisen.me
OK
#获取数据
127.0.0.1:6379> get hisen
"hisen.me"
127.0.0.1:6379> 
#停止数据库
hisen@hisen-server:/$ service redis stop
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to stop 'redis-server.service'.
Authenticating as: hisen,,, (hisen)
Password: 
==== AUTHENTICATION COMPLETE ===
hisen@hisen-server:/$ 
```

