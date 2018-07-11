---
title: Ubuntu安装RocketMQ - Quick Start
keywords: [安装RocketMQ,ubuntu安装RocketMQ,linux安装RocketMQ]
date: 2017/7/2 1:19
tags: [linux,rocketmq]
categories: linux
---
安装需求：
---
1. 64位的系统 Linux/Unix/Mac
2. 64bit JDK 1.8+;
3. Maven 3.2.x;
4. Git （一般自带）

如果还未安装jdk、maven建议查看教程:<a herf="/20170218-Ubuntu安装JDK8-Tomcat-简单教程/" target="_blank">点击查看</a>

Clone & Build
---
```
git clone -b develop https://github.com/apache/incubator-rocketmq.git
cd incubator-rocketmq
mvn -Prelease-all -DskipTests clean install -U
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] Apache RocketMQ 4.2.0-incubating-SNAPSHOT .......... SUCCESS [01:07 min]
[INFO] rocketmq-remoting 4.2.0-incubating-SNAPSHOT ........ SUCCESS [ 15.749 s]
[INFO] rocketmq-common 4.2.0-incubating-SNAPSHOT .......... SUCCESS [ 10.243 s]
[INFO] rocketmq-client 4.2.0-incubating-SNAPSHOT .......... SUCCESS [ 11.638 s]
[INFO] rocketmq-store 4.2.0-incubating-SNAPSHOT ........... SUCCESS [ 13.108 s]
[INFO] rocketmq-srvutil 4.2.0-incubating-SNAPSHOT ......... SUCCESS [  2.051 s]
[INFO] rocketmq-filter 4.2.0-incubating-SNAPSHOT .......... SUCCESS [  3.917 s]
[INFO] rocketmq-broker 4.2.0-incubating-SNAPSHOT .......... SUCCESS [  8.726 s]
[INFO] rocketmq-tools 4.2.0-incubating-SNAPSHOT ........... SUCCESS [  6.002 s]
[INFO] rocketmq-namesrv 4.2.0-incubating-SNAPSHOT ......... SUCCESS [  2.726 s]
[INFO] rocketmq-logappender 4.2.0-incubating-SNAPSHOT ..... SUCCESS [  3.514 s]
[INFO] rocketmq-openmessaging 4.2.0-incubating-SNAPSHOT ... SUCCESS [  2.668 s]
[INFO] rocketmq-example 4.2.0-incubating-SNAPSHOT ......... SUCCESS [  2.390 s]
[INFO] rocketmq-filtersrv 4.2.0-incubating-SNAPSHOT ....... SUCCESS [  2.145 s]
[INFO] rocketmq-test 4.2.0-incubating-SNAPSHOT ............ SUCCESS [  5.428 s]
[INFO] rocketmq-distribution 4.2.0-incubating-SNAPSHOT .... SUCCESS [ 27.002 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 03:07 min
[INFO] Finished at: 2017-07-02T01:35:40+08:00
[INFO] Final Memory: 60M/247M
[INFO] ------------------------------------------------------------------------
cd distribution/target/apache-rocketmq
```
Start Name Server
```
nohup sh bin/mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log
The Name Server boot success...
```
Start Broker
```
nohup sh bin/mqbroker -n localhost:9876 &
tail -f ~/logs/rocketmqlogs/broker.log 
The broker[%s, 172.30.30.233:10911] boot success...
```
Send & Receive Messages
```
Before sending/receiving messages, we need to tell clients the location of name servers. RocketMQ provides multiple ways to achieve this. For simplicity, we use environment variable NAMESRV_ADDR
export NAMESRV_ADDR=localhost:9876
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
SendResult [sendStatus=SEND_OK, msgId= ...

sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer 
ConsumeMessageThread_%d Receive New Messages: [MessageExt...
```
Shutdown Servers
```
sh bin/mqshutdown broker
The mqbroker(36695) is running...
Send shutdown request to mqbroker(36695) OK

sh bin/mqshutdown namesrv
The mqnamesrv(36664) is running...
Send shutdown request to mqnamesrv(36664) OK
```