---
title: java.lang.NoClassDefFoundError错误解决
keywords: [NoClassDefFoundError]
date: 2019/2/23 22:44
tags: [java]
categories: java
---
maven项目昨天遇到的一个错误：java.lang.NoClassDefFoundError

理论原因：JVM在编译的时候能找到调用方法或静态变量所在的类，但在运行的时候找不到此类而引发的错误。

真实原因：(仅针对当前问题)
项目A依赖公共服务C-1.0.1版本
项目B依赖项目A，B中dependencyManagement强制指定C-1.0.0

此时编译可以通过，运行时如果有用的C的新版本功能就会会出现上述问题

解决办法：对项目B中C的版本升级，使用1.0.1

扩展阅读：
https://blog.csdn.net/qq_27576335/article/details/77102385
https://blog.csdn.net/jamesjxin/article/details/46606307
