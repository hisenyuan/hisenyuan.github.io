---
title: rabbitMQ的安装和Demo
keywords: [rabbitMQ]
date: 2018/1/11 22:50
tags: [rabbitMQ]
categories: java
---
# 零、rabbitMQ介绍
[rabbitMQ详细介绍](http://blog.csdn.net/whycold/article/details/41119807)
1. 如果某个queue有多个订阅，消息分均分到消费者，而不是所有人都收到全部
2. 接收消息有ack(acknowledgment)机制，发送消息是没有这个机制的
3. 生产者将消息发送到Exchange（交换器），由Exchange将消息路由到一个或多个Queue中（或者丢弃）。
# 一、在ununtu上安装
## 1.1 安装
```
echo 'deb http://www.rabbitmq.com/debian/ testing main' | sudo tee /etc/apt/sources.list.d/rabbitmq.list

wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -

sudo apt-get update

sudo apt-get install rabbitmq-server
```
## 1.2 配置
```
# 打开管理页面功能
sudo rabbitmq-plugins enable rabbitmq_management
# 查看安装的插件
sudo rabbitmqctl list_users
# 查看用户
sudo rabbitmqctl list_users
# 新增管理员用户
sudo rabbitmqctl add_user admin admin
# 授予管理员权限
sudo rabbitmqctl set_user_tags admin administrator
# 管理页面地址，用刚设置的账户登录管理页面
```
 [http://127.0.0.1:15672](http://127.0.0.1:15672)
# 二、Java小Demo
## 2.1 遇到的问题
1. ACCESS_REFUSED - Login was refused using authentication mechanism PLAIN.
帐号密码错误，建议使用2.3配置的账户，guest账户不靠谱

2. connection error
ip或者port错误，确认信息是否正确，虚拟机的话看看端口映射是否正常

## 2.2 配置用户
```
# 添加普通用户
sudo rabbitmqctl add_user hisen hisen
# 添加权限
rabbitmqctl set_permissions -p "/"  hisen ".*" ".*" ".*"
# 列出用户权限
rabbitmqctl list_user_permissions hisen
```
## 2.3 添加maven依赖
```
<dependency>
  <groupId>com.rabbitmq</groupId>
  <artifactId>amqp-client</artifactId>
  <version>5.0.0</version>
</dependency>
```
## 2.4 发送端代码
<!--more-->
```
package com.hisen.jars.rabbitmq;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class Send {
  // 定义队列名字
  public static final String QUEUE_NAME = "hello";

  public static void main(String[] args) throws IOException, TimeoutException {
    // 创建连接工厂
    ConnectionFactory factory = new ConnectionFactory();
    factory.setHost("127.0.0.1");
    factory.setPort(5672);
    factory.setUsername("hisen");
    factory.setPassword("hisen");
    Connection connection = null;
    Channel channel = null;
    try {
      // 创建连接
      connection = factory.newConnection();
      // 创建信道
      channel = connection.createChannel();
      // 声明一个队列：名称、持久性的（重启仍存在此队列）、非私有的、非自动删除的
      channel.queueDeclare(QUEUE_NAME, false, false, false, null);
      for (int i = 0; i < 10; i++) {
        // 定义消息内容
        String message = "Hello World - " + i;
        // 通过信道发布内容
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
        System.out.println("send : " + message);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } catch (TimeoutException e) {
      e.printStackTrace();
    } finally {
      if (null!=channel) {
        channel.close();
      }
      if (null!= connection ) {
        connection.close();
      }
    }
  }
}
```
## 2.5 接收端代码
```
package com.hisen.jars.rabbitmq;

import com.rabbitmq.client.AMQP.BasicProperties;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;
import java.io.IOException;
import java.util.concurrent.TimeoutException;
import org.joda.time.DateTime;

public class Receive{
  // 定义队列名字
  public static final String QUEUE_NAME = "hello";

  public static void main(String[] args) {
    // 创建连接工厂
    ConnectionFactory factory = new ConnectionFactory();
    factory.setHost("127.0.0.1");
    factory.setUsername("hisen");
    factory.setPassword("hisen");
    try {
      // 创建连接
      Connection connection = factory.newConnection();
      // 创建信道
      Channel channel = connection.createChannel();
      // 声明一个队列：名称、持久性的（重启仍存在此队列）、非私有的、非自动删除的
      channel.queueDeclare(QUEUE_NAME, false, false, false, null);
      System.out.println("watting for message");

      /* 定义消费者 */
      DefaultConsumer consumer = new DefaultConsumer(channel) {
        @Override
        public void handleDelivery(String consumerTag, Envelope envelope,
            BasicProperties properties, byte[] body)
            throws IOException {
          String message = new String(body, "UTF-8");
          System.out.println("Received time:" + new DateTime().toString("yyyy-MM-dd HH:mm:ss:SSS EE")+ " the message is -> " + message);
        }
      };
      // 将消费者绑定到队列，并设置自动确认消息（即无需显示确认，如何设置请慎重考虑）
      channel.basicConsume(QUEUE_NAME, true, consumer);
    } catch (IOException e) {
      e.printStackTrace();
    } catch (TimeoutException e) {
      e.printStackTrace();
    }
  }
}
```