---
title: 为什么要使用队列 - Java
keywords: [队列,为什么要使用队列,队列的使用场景]
date: 2017/8/1 16:03
tags: [队列,java]
categories: java
---
一、java中的队列：Queue接口
--
Queue接口与List、Set同一级别，都是继承了Collection接口。LinkedList实现了Queue接口。Queue接口窄化了对LinkedList的方法的访问权限（即在方法中的参数类型如果是Queue时，就完全只能访问Queue接口所定义的方法 了，而不能直接访问 LinkedList的非Queue的方法），以使得只有恰当的方法才可以使用。BlockingQueue 继承了Queue接口。

队列是一种数据结构．它有两个基本操作：在队列尾部加入一个元素，和从队列头部移除一个元素（注意不要弄混队列的头部和尾部）就是说，队列以一种先进先出的方式管理数据，如果你试图向一个 已经满了的阻塞队列中添加一个元素或者是从一个空的阻塞队列中移除一个元索，将导致线程阻塞．在多线程进行合作时，阻塞队列是很有用的工具。工作者线程可以定期地把中间结果存到阻塞队列中而其他工作者线程把中间结果取出并在将来修改它们。队列会自动平衡负载。如果第一个线程集运行得比第二个慢，则第二个 线程集在等待结果时就会阻塞。如果第一个线程集运行得快，那么它将等待第二个线程集赶上来。下表显示了jdk1.5中的阻塞队列的操作：

| 排序方法 | 平均情况 | 最好情况 |
|:-----|:-----|:-----|
| add  | 增加一个元素 |  如果队列已满，则抛出一个IllegalSlabEepeplian异常 |
| remove | 移除并返回队列头部的元素 | 如果队列为空，则抛出一个NoSuchElementException异常 |
| element | 返回队列头部的元素 | 如果队列为空，则抛出一个NoSuchElementException异常 |
| offer | 添加一个元素并返回true | 如果队列已满，则返回false |
| poll | 移除并返问队列头部的元素  | 如果队列为空，则返回null |
| peek | 返回队列头部的元素 | 如果队列为空，则返回null |
| put | 返回队列头部的元素 | 如果队列满，则阻塞 |
| take | 返回队列头部的元素 | 如果队列为空，则阻塞 |

二、消息队列
--
介绍：

消息队列中间件是分布式系统中重要的组件，主要解决应用耦合，异步消息，流量削锋等问题。

实现高性能，高可用，可伸缩和最终一致性架构。

是大型分布式系统不可缺少的中间件。

目前在生产环境，使用较多的消息队列有ActiveMQ，RabbitMQ，ZeroMQ，Kafka，MetaMQ，RocketMQ等。

场景：异步处理、应用解耦、流量削锋、日志处理

---

以上就是关于为什么要使用队列的大致说明

---
参考：
1. <a href="http://blog.csdn.net/kangbin825/article/details/53966618" target="_blank">java中队列的使用</a>
2. <a href="http://blog.csdn.net/he90227/article/details/50800646" target="_blank">消息队列的使用场景</a>