---
title: IDEA failed to create jvm:error code -1
keywords: [IDEA failed to create jvm:error code -1,jvm:error code -1]
date: 2017/3/3 18:00
tags: [java]
categories: java
---
今天先更改了 idea64.exe.vmoptions 这个配置文件

一直么有重启，后来就安装了个插件重启一下，结果就泪崩了

一直出现这个错误
<img src="http://wx4.sinaimg.cn/mw690/b2e389b6gy1fd9suz8h9dj20db06ujr9.jpg"  alt="IDEA failed to create jvm:error code -1,jvm:error code -1" />
总以为是环境变量配置的问题，或者是文件损坏了什么

重启，重装jdk，重新配置什么都试过，不管用。

后来替换了配置文件就好了！！！

解决方案
---

配置文件路径：
```
\IDEA HOME\bin\idea64.exe.vmoptions
或者
\IDEA HOME\bin\idea.exe.vmoptions
```
默认配置文件内容如下：

32bit
```
-server
-Xms128m
-Xmx512m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
```
64bit
```
-Xms128m
-Xmx750m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
```

by the way:

IDEA 写博客真是舒服啊~

完全不用切换来切换去的！