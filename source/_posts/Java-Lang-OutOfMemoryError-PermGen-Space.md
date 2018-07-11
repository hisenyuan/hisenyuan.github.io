---
title: 'IDEA - Java.Lang.OutOfMemoryError: PermGen Space'
keywords: [Java.Lang.OutOfMemoryError: PermGen Space,PermGen Space,tomcat PermGen Space]
tags: [tomcat,idea]
date: 2017-02-22 11:18:43
categories: java
---
Java.Lang.OutOfMemoryError: PermGen Space

Tomcat只分配了非常小的PermGen内存，这里重新设置一下

直接在配置tomcat的时候,在VM options填入:
```
-XX:PermSize=97m -XX:MaxPermSize=256m
```