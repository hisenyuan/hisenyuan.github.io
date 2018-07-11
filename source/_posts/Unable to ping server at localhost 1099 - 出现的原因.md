---
title: Unable to ping server at localhost 1099 - 出现的原因
keywords: [Unable to ping server at localhost 1099]
date: 2017/3/30 20:05
tags: [idea,java]
categories: idea
---
之前老是出现
```
Application Server was not connected before run configuration stop, 
reason: Unable to ping server at localhost:1099
```
我遇到这个问题一般是这些原因：

1. 这个端口被占用，一般进程管理把所有java进程杀了可以解决
2. 由于在IDEA中错误的给tomcat添加了参数,比如下面这个。去掉即可

这是下VM option中加了：-URIEncoding=UTF-8
```
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
Unrecognized option: -URIEncoding=UTF-8
```