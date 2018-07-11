---
title: Ubuntu虚拟机安装MySQL并且开启root远程访问
keywords: [ubuntu mysql 远程访问,buntu虚拟机安装MySQL并且开启root远程访问]
date: 2017/3/11 0:44
tags: [mysql]
categories: sql
---
安装mysql很简单，关键是开启这个远程很坑！！！

一、安装
---

1.安装
```
sudo apt-get install mysql-server
```
等待完成即可，过程中需要设置密码

2.查看是否成功
```
sudo netstat -tap | grep mysql
```
3.登陆mysql
```
mysql -u root -p
```
这条命令回车之后需要输入mysql密码

二、开启远程访问
---
```
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
#找到bind-address=127.0.0.1直接注释
$ mysql -u root -p -h
#登陆mysql
mysql> use mysql;
#使用mysql这个库
mysql> GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "hisen";
#把root用户改成可以在任何ip上登陆，并且密码为：hisen
mysql> flush privileges;
#刷新
```
重启：service mysql restart

接下来就可以在navicat里面连接了

三、注意事项
---
因为在网上找的很多教程，都是说改这个配置文件：这个是错误的
```
/etc/mysql/my.cf
```
如果是通过apt-get方式安装的，默认的是第二步那个配置文件