---
title: Jedis - Software caused connection abort:recv failed
keywords: [Jedis - Software caused connection abort:recv failed,recv failed]
date: 2017/4/11 13:23
tags: [jedis,redis]
categories: java
---
在使用jedis的时候出现这个问题：

**redis.clients.jedis.exceptions.JedisConnectionException:java.net.SocketException:Software caused connection abort: recv failed**

我是windows上java运行，然后redis是在虚拟机的，通过映射访问

解决办法：
---

编辑redis配置文件：
```
sudo vi /etc/redis/redis.conf
```
找到
```
bind 127.0.0.1
```
改成
```
bind 0.0.0.0
```
改完之后重启redis
```
service redis restart
```
即可。这跟mysql一样，允许任何ip连接！