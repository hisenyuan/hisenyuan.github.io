---
title: java编译错误：程序包javax.servlet不存在javax.servlet.*
keywords: [java编译错误,不存在javax.servlet]
tags: [java]
date: 2017-02-17 15:59:19
categories: java
---

之前用myeclipse开发的项目

今天导入到IDEA中去，发现编译出问题

ava编译错误：程序包javax.servlet不存在javax.servlet.*

---

原因大概是myeclipse中可以选择Java EE项目

而idea没有，缺少 servlet-api.jar 这个jar包

---
解决办法：
1. 复制tomcat文件夹下lib--->servlet-api.jar 这个jar包
2. 添加到IDEA项目中去：粘贴到External Library目录下

即可