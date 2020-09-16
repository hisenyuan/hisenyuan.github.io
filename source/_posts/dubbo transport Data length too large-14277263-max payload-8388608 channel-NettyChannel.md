---
title: dubbo transport Data length too large-14277263-max payload-8388608 channel-NettyChannel
keywords: [dubbo,NettyChannel,Data length too large,8388608]
date: 2019/3/7 20:01
tags: [java,netty,dubbo]
categories: java
---
# 一、报错信息
```
com.alibaba.dubbo.remoting.transport.ExceedPayloadLimitException: Data length too large: 14277263, max payload: 8388608, channel: NettyChannel [channel=[id: 0x12a13c8f, /172.0.0.1:49402 => /172.0.0.2:23888]]
```
# 二、报错原因
dubbo默认使用Netty传输协议
并且默认的大小限制为：默认为8M，即8388608

# 三、解决办法
1. 修改接口
出现这种情况是因为一个接口查某个表的所有数据(几万条)
一般这种接口肯定是需要分页的

2. 更改配置信息
在dubbo.properties 中增加如下
```
dubbo.protocol.dubbo.payload=11557050
```
