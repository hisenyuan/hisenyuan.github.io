---
title: DockerFile - 创建java开发环境镜像
keywords: [DockerFile,docker]
date: 2018/1/11 22:38
tags: [docker]
categories: linux
---

使用ubuntu官方发布的docker镜像进行二次修改

这是一个菜鸟的脚本，执行命令应该是使用 & 连接，一个RUN命令搞定
```
FROM    ubuntu
MAINTAINER      Fisher "hisenyuan@gmail.com"
RUN     /bin/echo 'root:hisen' |chpasswd
RUN     useradd hisen
RUN     /bin/echo 'hisen:hisen' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
# 显示系统位数
RUN		uname -p
# 清空源 
RUN     echo "" > /etc/apt/sources.list
# 更换为阿里云源
RUN		echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list
RUN		echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list
# 更新源
RUN		apt-get update
# 安装软件
RUN     apt-get -y install vim
RUN     apt-get -y install curl
RUN     apt-get -y install wget
RUN     apt-get -y install net-tools
RUN     apt-get -y install iputils-ping
RUN     apt-get -y install git
# 创建软件文件夹
RUN     mkdir -p /usr/hisen/soft/java
RUN     mkdir -p /usr/hisen/soft/tomcat
RUN     mkdir -p /usr/hisen/soft/maven
RUN     mkdir -p /usr/hisen/soft/download
# 添加本地软件包到指定文件夹（会自动解压，软件压缩包必须放在docker同级目录）
ADD jdk-8u151-linux-x64.tar.gz /usr/hisen/soft/java/
ADD apache-tomcat-8.5.24.tar.gz /usr/hisen/soft/tomcat/
ADD apache-maven-3.5.2-bin.tar.gz /usr/hisen/soft/maven/

# 配置环境变量
# java
ENV JAVA_HOME=/usr/hisen/soft/java/jdk1.8.0_151
ENV JRE_HOME=$JAVA_HOME/jre
ENV CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
ENV PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
# tomcat
ENV CATALINA_HOME=/usr/hisen/soft/tomcat/apache-tomcat-8.5.24
ENV CLASSPATH=.:$JAVA_HOME/lib:$CATALINA_HOME/lib
ENV PATH=$PATH:$CATALINA_HOME/bin
# maven
ENV MAVEN_HOME=/usr/hisen/soft/maven/apache-maven-3.5.2
ENV MAVEN_OPTS="-Xms256m -Xmx512m"
ENV PATH=${MAVEN_HOME}/bin:$PATH

# 监听端口
EXPOSE  22
EXPOSE  80
EXPOSE  8080
CMD     /usr/sbin/sshd -D
```