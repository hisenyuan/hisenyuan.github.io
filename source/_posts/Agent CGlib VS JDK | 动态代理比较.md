---
title: Agent CGlib VS JDK | 动态代理比较
keywords: [动态代理,CGlib Agent,JDK Agent,Agent]
date: 2019/3/30 00:16
tags: [java,jdk]
categories: java
---
# 一、代理简介
## 1.1 代理是什么
举个例子：快到吃饭的点了，你有两种选择：1)自己做,2)叫外卖
1.1.1 自己做你嫌麻烦，那就叫外卖，只管收外卖其它不关注(解耦);
1.1.2 自己做的不好吃，那厨师上门，厨师给你增强菜的味道(增强);

官方定义：对其他对象提供一种代理以控制对这个对象的访问。

## 1.2 使用场景是什么
1.2.1 设计模式中有一个设计原则是开闭原则，是说对修改关闭对扩展开放，我们在工作中有时会接手很多前人的代码，里面代码逻辑让人摸不着头脑(sometimes the code is really like shit)，这时就很难去下手修改代码，那么这时我们就可以通过代理对类进行增强。

1.2.2 我们在使用RPC框架的时候，框架本身并不能提前知道各个业务方要调用哪些接口的哪些方法 。那么这个时候，就可用通过动态代理的方式来建立一个中间人给客户端使用，也方便框架进行搭建逻辑，某种程度上也是客户端代码和框架松耦合的一种表现。

1.2.3 Spring的AOP机制就是采用动态代理的机制来实现切面编程。

# 二、CGlib VS JDK 两种方式实现代理
## 2.1 差异
2.1.1 JDK：只能代理接口
2.1.2 CGlib：直接代理类

## 2.2 简单demo
<!--more-->
完整代码：https://github.com/hisenyuan/IDEAPractice/tree/master/src/main/java/com/hisen/jdk/agent
```java
public static void main(String[] args) {
    System.out.println("===CGlibAgentTest===");
    cGlibAgentTest();

    System.out.println("\n===JDKDynamicAgentTest===");
    jdkDynamicAgentTest();
}

private static void cGlibAgentTest() {
    CGlibAgent cGlibAgent = new CGlibAgent();
    Apple apple = (Apple) cGlibAgent.getInstance(Apple.class);
    apple.show();

    System.out.println();

    Orange orange = (Orange) cGlibAgent.getInstance(Orange.class);
    orange.show();
}

private static void jdkDynamicAgentTest() {
    // must be return interface
    Fruit apple = (Fruit) DynamicAgent.agent(Fruit.class, new Apple());
    apple.show();

    System.out.println();

    Fruit orange = (Fruit) DynamicAgent.agent(Fruit.class, new Orange());
    orange.show();
}
//===CGlibAgentTest===
//-> before invoking
//Apple show method is invoked
//-> after invoking
//
//-> before invoking
//    Orange show method is invoked
//-> after invoking
//
//===JDKDynamicAgentTest===
//-> before invoking
//    Apple show method is invoked
//-> after invoking
//
//-> before invoking
//    Orange show method is invoked
//-> after invoking
```

# 三、参考连接
http://www.cnblogs.com/puyangsky/p/6218925.html
https://blog.csdn.net/u011784767/article/details/78281384
