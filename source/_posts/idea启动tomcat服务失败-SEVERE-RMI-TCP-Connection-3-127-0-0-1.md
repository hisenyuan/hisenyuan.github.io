---
title: 'idea启动tomcat服务失败 SEVERE [RMI TCP Connection(3)-127.0.0.1]'
keywords: [[RMI TCP Connection(3)-127.0.0.1]]
tags: [idea,tomcat]
date: 2017-02-15 09:53:05
categories: java
---
工程是从eclipse生成的，用idea开发。

重复了一遍以往正常的不能再正常了的导入配置，结果遇到了如下问题：

SEVERE [**RMI TCP Connection(3)-127.0.0.1**] 
```
15-Feb-2017 11:05:25.993 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.catalina.core.StandardContext.listenerStart Skipped installing application listeners due to previous error(s)
```

删除导入项目中的web.xml文件
---
因为idea要用的东西自己会自动生成

然后就搞定了这个纠结我一天的问题


我是百度搜索这个错误：**RMI TCP Connection(3)-127.0.0.1**

得到的答案，感谢10100：<a href="http://www.cnblogs.com/A-QBlog/p/5865578.html" target="_blank">查看原文</a>