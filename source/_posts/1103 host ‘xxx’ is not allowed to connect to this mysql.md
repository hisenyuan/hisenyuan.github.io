---
title: 1103 host 'xxx' is not allowed to connect to this mysql
keywords: [1103 host '' is not allowed to connect to this mysql]
date: 2017/3/11 1:21
tags: [mysql]
categories: java
---
出现原因： 
这是由于mysql服务端root用户所对应的客户端权限设置问题。

默认所对应的客户端地址只有localhost（也就是服务端的机器），

我们目的是任何地址都可以用root访问mysql服务端。 

解决办法：
```
$ mysql -u root -p
#进入mysql交互界面
mysql> use mysql;
#使用mysql这个库
mysql> grant all privileges on *.* to 'root'@'%' identified by 'hisen'; 
#让root可以在任何ip登陆，密码为：hisen
mysql> flush privileges; 
#刷新
mysql> exit;
#退出
$ service mysql restart
#重启mysql
```