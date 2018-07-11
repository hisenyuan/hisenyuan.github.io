---
title: Spring - IOC和AOP的原理
keywords: [ioc,aop,ioc aop原理]
date: 2017/11/20 23:11
tags: [spring]
categories: java
---
## 简单理解IOC和AOP
### IOC：Inversion of Control，依赖倒置
#### 生活场景助记

如有有天你想喝一瓶矿泉水，你可以去小区便利店，告诉老板你要买矿泉水，然后老板卖给你。

但是你可能需要想这下雨天怎么去小卖部？是否要带伞？去了之后是否有我想要的水等一系列问题。

解决这个问题：
1. 使用外卖！到平台注册，告诉平台你需要什么水。
2. 平台给你送到家，你只管付钱拿到水之后直接喝，不用考虑上述问题。


是不是和Spring的做法很类似呢？Spring就是小卖部，你就是A对象，水就是B对象
第一：在Spring中声明一个类：A
第二：告诉Spring，A需要B
<!--more-->
假设A是UserAction类，而B是UserService类
```
<bean id="userService" class="org.leadfar.service.UserService"/>
<bean id="documentService" class="org.leadfar.service.DocumentService"/>
<bean id="orgService" class="org.leadfar.service.OrgService"/>

<bean id="userAction" class="org.leadfar.web.UserAction">
     <property name="userService" ref="userService"/>
</bean>
```
在Spring这个商店（工厂）中，有很多对象/服务：userService,documentService,orgService

也有很多会员：userAction等等

声明userAction需要userService即可，

Spring将通过你给它提供的通道主动把userService送上门来，因此UserAction的代码示例类似如下所示：
```
package org.leadfar.web;
public class UserAction{
     private UserService userService;
     public String login(){
          userService.valifyUser(xxx);
     }
     public void setUserService(UserService userService){ 
          this.userService = userService;
     }
}
```
在这段代码里面，你无需自己创建UserService对象（Spring作为背后无形的手，把UserService对象通过你定义的setUserService()方法把它主动送给了你，这就叫依赖注入！）

Spring依赖注入的实现技术是：**动态代理**

### AOP:Aspect Oriented Programming,面向切面编程
你只要做你关注的事情，其他的事情一概不管，让AOP帮你去做

你可以灵活组合各种杂七杂八的事情交给AOP去做，而不会干扰你关注的事情。

从Spring的角度看，AOP最大的用途就在于提供了事务管理的能力。

事务管理就是一个关注点，你的正事就是去访问数据库，而你不想管事务（太烦）

所以，Spring在你访问数据库之前，自动帮你开启事务，当你访问数据库结束之后，自动帮你提交/回滚事务！

## ICO 和 AOP的原理
### IoC的实现形式有两种：
1. 依赖查找：容器提供回调接口和上下文环境给组件。EJB和Apache Avalon都是使用这种方式。     
2. 依赖注入：组件不做定位查询，只是提供普通的Java方法让容器去决定依赖关系。
容器全权负责组件的装配，它会把符合依赖关系的对象通过JavaBean属性或者构造子传递给需要的对象。

通过JavaBean属性注射依赖关系的做法称为设值方法注入（Setter Injection）；

将依赖关系作为构造子参数传入的做法称为构造子注入（Constructor Injection）。

### AP实现有多种方案，主要包括：
1. AspectJ (TM)：创建于Xerox PARC. 有近十年历史，技术成熟。但其过于复杂；破坏封装；而且需要专门的Java编译器，易用性较差。
2. 动态代理AOP：使用JDK提供的动态代理API或字节码Bytecode处理技术来实现。基于动态代理API的具体项目有： JBoss 4.0 JBoss 4.0服务器。
3. 基于字节码的AOP，例如：Aspectwerkz、CGlib、Spring等。
## 后期准备补充
用过spring的朋友都知道spring的强大和高深，都觉得深不可测，

其实当你真正花些时间读一读源码就知道它的一些技术实现其实是建立在一些最基本的技术之上而已；
例如：
1. AOP(面向方面编程)的实现是建立在CGLib提供的类代理和jdk提供的接口代理
2. IOC(控制反转)的实现建立在工厂模式、Java反射机制和jdk的操作XML的DOM解析方式.
## 参考
1. https://www.cnblogs.com/damowang/p/4305107.html
2. http://blog.csdn.net/linxijun120903/article/details/56487073