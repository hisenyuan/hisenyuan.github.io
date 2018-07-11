---
title: zookeeper & dubbo搭建 - 在IDEA上运行阿里巴巴Dubbo-demo
keywords: [zookeeper dubbo搭建,dubbo搭建,dubbo-demo，dubbo-admin]
date: 2017/4/19 10:17
tags: [dubbo,zookeeper,java]
categories: java
---
<img src="http://wx3.sinaimg.cn/mw690/b2e389b6ly1ferrzk70zwj20nb0diadw.jpg" />

IDEA上搭建dubbo服务的简单过程
---
只是简单的让例子在IntelliJ IDEA跑起来

目前是最新的版本：2.5.4-SNAPSHOT

本文档更新时间：2017年04月19日01:08:02


一 、安装zookeeper
---
参考链接：<a href="http://hisen.me/20170224-Ubuntu-16-LTS-%E5%AE%89%E8%A3%85zookeeper%E5%B9%B6%E5%BC%80%E6%9C%BA%E5%90%AF%E5%8A%A8/" target="_blank">ubuntu apt-get安装zookeeper</a>

二、Idea clone本项目
---
导出项目之后，配置一下tomcat，添加dubbo-admin：war到tomcat中

项目github地址：<a href="https://github.com/hisen-yuan/dubbo" target="_blank">https://github.com/hisen-yuan/dubbo</a>

三、启动tomcat，即可访问dubbo管理后台
---
默认账号：root

默认密码：root

四、启动服务提供者&消费者demo
---

1. 修改dubbo-demo-consumer配置文件中的注册中心地址
```
/dubbo/dubbo-demo/dubbo-demo-consumer/src/test/resources/dubbo.properties
```

```
#dubbo.registry.address=multicast://224.5.6.7:1234
#使用本地的zookeeper做注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

2. 修改ubbo-demo-provider配置文件中的注册中心地址
```
/dubbo/dubbo-demo/dubbo-demo-provider/src/test/resources/dubbo.properties
```

```
#dubbo.registry.address=multicast://224.5.6.7:1234
#使用本地的zookeeper做注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

3. 分别启动dubbo-demo下ubbo-demo-provider、dubbo-demo-consumer下的测试方法

即可在后台看到有服务在运行
