---
title: java.sql.SQLException Connections could not be acquired from the underlying database
keywords: [java,c3p0]
date: 2017/6/2 16:42
tags: [java]
categories: java
---
java.sql.SQLException: Connections could not be acquired from the underlying database

这个错误出现的原因有的说是因为jdbc配置文件写错了

正确的写法是：
```
c3p0.driverClass=com.mysql.jdbc.Driver
c3p0.jdbcUrl=jdbc:mysql://localhost:3306/test
c3p0.user=root
c3p0.password=root
```
而不是
```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/test
user=root
password=root
```

但是我遇到的不是如此

后来发现是因为maven出问题了，jar包没有加载进项目

这是在starkoverflow上看到的回答
```
In my case it was problem that c3p0-0.9.2.1.jar file was not copied by maven into src/main/webapp/web-inf/libs
```
最后maven重新install之后就解决了