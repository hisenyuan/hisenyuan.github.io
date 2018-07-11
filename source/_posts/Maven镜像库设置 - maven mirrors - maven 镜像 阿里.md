---
title: Maven镜像库设置 - maven mirrors - maven 镜像 阿里
keywords: [maven国内镜像,maven国内镜像 阿里云	,maven mirror,maven镜像设置]
date: 2017/4/12 17:28
tags: [maven]
categories: maven
---

配置文件：
```
yourPath\maven-3.3.9\conf\settings.xml
```
找到里面的，添加镜像即可
```
<mirrors>
</mirrors>
```
<mirrorOf></mirrorOf>

这里写的是被镜像的ID

如果写成：* (星号)

所有的请求都会到这个镜像上，包括各种本地库

注意：千万不要配成**<mirrorOf>*</mirrorOf>**

否则内网的仓库或者你配的镜像里面没有一下jar包的时候不会去别的地方搜索
```
<!--阿里云：速度挺快-->
<mirror>
	<id>nexus-aliyun</id>
	<mirrorOf>central</mirrorOf>
	<name>Nexus aliyun</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>

<!--谷歌：北京速度不错-->
<mirror>
  <id>google-maven-central</id>
  <name>Google Maven Central</name>
  <url>https://maven-central.storage.googleapis.com</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```