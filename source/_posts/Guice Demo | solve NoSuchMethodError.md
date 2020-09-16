---
title: Guice Demo | solve NoSuchMethodError
keywords: [google Guice,Guice,Guice demo,guice java.lang.NoSuchMethodError,guice java.lang.NullPointerException]
date: 2019/4/24 18:30
tags: [java]
categories: java
---
# 一、Guice简介
Google公司的Bob lee开发的轻量级IoC容器，其特点是：
1. 速度快，号称是spring的100倍速度
2. 无配置文件，实用JDK5.0的annotation描述组件依赖，简单，而且有编译器检查和重构支持
3. 简单，代码量很少

# 二、简单样例
1. 详细代码：https://github.com/hisenyuan/IDEAPractice/tree/master/src/main/java/com/hisen/jars/guice
2. 依赖
```xml
        <dependency>
            <groupId>com.google.inject</groupId>
            <artifactId>guice</artifactId>
            <version>4.2.2</version>
        </dependency>
```
3. 测试类
```java
public class HelloApp extends BaseServer {
    @Inject
    private HelloServiceImpl hello;

    @Test
    public void testSayHello() {
        // 方式一
        Injector injector = Guice.createInjector();
        HelloService helloService = injector.getInstance(HelloService.class);
        helloService.sayHello("hisen");

        // 方式二 其实是在BaseServer中做了方式1的事情 【类似Spring的方式】
        hello.sayHello("1");
    }
}
```

# 三、解决问题
```java
java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;Ljava/lang/Object;)V
```
github有人遇到同样的问题：https://github.com/SeleniumHQ/selenium/issues/3880

把本地的guava版本由19.0改为21.0成功解决问题
