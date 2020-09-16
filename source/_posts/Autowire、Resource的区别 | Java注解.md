---
title: Autowire、Resource的区别 | Java注解
keywords: [注解,java,spring,Autowire,Resource]
date: 2019/7/22 21:00
tags: [java]
categories: java
---
# 零、本文背景
项目中看到有一个缓存接口存在多个实现类,
但是在代码中使用@Resource注解注入,
之前有了解过@Autowire @Resource的区别,
于是就尝试着搜索@@Resource，于是就有本文的总结了。

# 一、两个注解
## 1.1 @Autowire
1.1.1 Spring开发;
1.1.2 按照type来注入;

## 1.2 @Resource
1.2.1 JDK开发；
1.2.2 按照名称注入，若无，则按type来注入(未指定name的情况下);

# 二、后记
1. 做事情要有计划，得主动;
2. 需要想清楚自己想要什么样的生活，然后朝着目标奋斗;
3. 使用现成的代码尽量搞清楚来龙去脉，可以学习知识，更能避免被坑;
4. 每天的学习时间需要保证，坚持很重要;
