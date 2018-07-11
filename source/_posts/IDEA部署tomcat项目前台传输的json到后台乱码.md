---
title: IDEA部署tomcat项目前台传输的json到后台乱码 - IDEA乱码解决
keywords: [idea tomcat json乱码,idea乱码,tomcat乱码，idea tomcat乱码]
date: 2017/3/30 21:44
tags: [idea,java]
categories: idea
---
idea涉及编码的地方都改了
主要是编译时候的编码，tomcat的编码，以及idea配置里面的编码

一、idea配置文件
```
\HOME\IntelliJ IDEA 2016.3.4\bin\idea64.exe.vmoptions
```
增加一行：-Dfile.encoding=UTF-8

二、编译参数
```
File -> Settings -> Build, Execution, Deployment
-> Compiler -> Java Compiler -> Addition command line parameters
```
在空格里面添加：-encoding utf-8
<img src="http://wx1.sinaimg.cn/mw690/b2e389b6ly1fe57tlrhrhj20n10fa3zl.jpg" />
三、工程编码
```
File -> Settings -> Editor -> File Encodings
```
此页面三个地方都选择UTF-8
<img src="http://wx3.sinaimg.cn/mw690/b2e389b6ly1fe57tmo745j20qw0jbmys.jpg" />
四、tomcat参数
```
Run/debug Configuration tomcat
```
VM options：-Dfile.encoding=UTF-8
<img src="http://wx3.sinaimg.cn/mw690/b2e389b6ly1fe57tn7v93j20k60atmxt.jpg" />