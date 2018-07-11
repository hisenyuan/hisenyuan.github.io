---
title: Linux-Ubuntu安装:JDK & Tomcat & Maven - 简单教程
keywords: [Linux-Ubuntu安装:JDK & Tomcat & Maven]
tags: [linux]
date: 2017-02-18 15:04:57
categories: linux
---

在网速搜索很多教程，感觉写的都太难了我去

准备工作：
1. 下载JDK，并解压(选择适合自己的版本：<a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html" target="_blank">地址</a>)
2. 下载Tomcat，并解压(选择适合自己的版本：<a href="http://tomcat.apache.org/download-80.cgi" target="_blank">地址</a>)
3. 下载Maven，并解压(选择适合自己的版本：<a href="http://maven.apache.org/download.cgi" target="_blank">地址</a>)

目录约定：
1. java路径：/usr/hisen/soft/java/jdk8
2. tomcat路径：/usr/hisen/soft/tomcat/tomcat8
3. maven路径：/usr/hisen/soft/maven/maven-3.5.0

说明以上路径都是解压之后的，请解压之后自行重命名文件夹等工作

<!--more-->

下面开始配置环境变量：
```
sudo vi /etc/profile
```
底部添加：
```
#java的环境变量配置
export JAVA_HOME=/usr/hisen/soft/java/jdk8
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

#tomcat的环境变量配置
export CATALINA_HOME=/usr/hisen/soft/tomcat/tomcat8
export CLASSPATH=.:$JAVA_HOME/lib:$CATALINA_HOME/lib
export PATH=$PATH:$CATALINA_HOME/bin

#maven环境变量
export MAVEN_HOME=/usr/hisen/soft/maven/maven-3.5.0
export MAVEN_OPTS="-Xms256m -Xmx512m"
export PATH=${MAVEN_HOME}/bin:$PATH
```
让刚刚的配置生效：
```
source /etc/profile
```
查看maven
```
mvn -v
```
查看java版本
```
java -version
```
如果还是默认的OpenJDK
```
sudo update-alternatives --install /usr/bin/java java /usr/hisen/soft/java/jdk8/bin/java 300  
sudo update-alternatives --install /usr/bin/javac javac /usr/hisen/soft/java/jdk8/bin/javac 300

#选择默认JDK即可
sudo update-alternatives --config java
```
进入tomcat的bin目录
```
sudo vi catalina.sh
```
顶部添加
```
#让tomcat知道java在哪里
JAVA_HOME=/usr/hisen/soft/java/jdk8
JRE_HOME=$JAVA_HOME/jre
```

之后进入在tomcat bin目录执行
```
hisen@hisen:/usr/hisen/soft/tomcat/tomcat8/bin$ sudo sh startup.sh
Using CATALINA_BASE:   /usr/hisen/soft/tomcat/tomcat8
Using CATALINA_HOME:   /usr/hisen/soft/tomcat/tomcat8
Using CATALINA_TMPDIR: /usr/hisen/soft/tomcat/tomcat8/temp
Using JRE_HOME:        /usr/hisen/soft/java/jdk8/jre
Using CLASSPATH:       /usr/hisen/soft/tomcat/tomcat8/bin/bootstrap.jar:/usr/hisen/soft/tomcat/tomcat8/bin/tomcat-juli.jar
Tomcat started.
```
这样就启动了tomcat！！！！
---

注意
---
如果没有在 catalina.sh 添加java路径，会报错
```
hisen@hisen-VirtualBox:/usr/hisen/soft/tomcat/tomcat8/bin$ sudo sh startup.sh
Neither the JAVA_HOME nor the JRE_HOME environment variable is defined
At least one of these environment variable is needed to run this program
```