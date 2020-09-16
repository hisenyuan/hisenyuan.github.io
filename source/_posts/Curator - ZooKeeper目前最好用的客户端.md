---
title: Curator - 封装分布式锁等 | ZooKeeper目前最好用的客户端
keywords: [Curator ZooKeeper,zookeeper client,zk分布式锁]
date: 2019/4/25 18:30
tags: [java]
categories: java
---
# 一、什么是Curator
Apache Curator is a Java/JVM client library for Apache ZooKeeper, a distributed coordination service.
It includes a highlevel API framework and utilities to make using Apache ZooKeeper much easier and more reliable.
It also includes recipes for common use cases and extensions such as service discovery and a Java 8 asynchronous DSL.

特别说明：Guava is to Java What Curator is to ZooKeeper

# 二、常见用法
后期补上自己的一些demo，其实官方文档已经介绍很全了
http://curator.apache.org/getting-started.html

# 三、其它说明
了解这个客户端是在《从Paxos到Zookeeper:分布式一致性原理与实战》这本书里面看到的([书单](/booklist/))
Curator号称是世界上最好用的zk客户端，相比zkClinet来说拥有更好的封装
让我想起Redisson和Jedis的模样

昨天一个做移动端的前同事截图问我那些代码什么意思，用的就是Curator封装的分布式锁

相比于Redis分布式存在超时问题，zookeeper分布式锁利用临时节点可以避免

目前dubbo master上使用的是Curator 4.0.1
