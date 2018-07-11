---
title: 'BeanCreationException: Error creating bean with name ''xxxService'''
keywords: []
tags: []
date: 2017-02-23 11:02:56
categories:
---
出现的问题：
```
[platform] ERROR 2017-02-22 17:46:05,756 [RMI TCP Connection(4)-127.0.0.1] org.springframework.web.context.ContextLoader.() | Context initialization failed org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'pmTranLimitLiteServiceImpl': Injection of resource dependencies failed; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [com.msds.zkutil.ZkLockFactory] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@javax.annotation.Resource(mappedName=, shareable=true, description=, name=, type=class java.lang.Object, authenticationType=CONTAINER, lookup=)}
	at org.springframework.context.annotation.CommonAnnotationBeanPostProcessor.postProcessPropertyValues(CommonAnnotationBeanPostProcessor.java:306) ~[spring-context-3.2.4.RELEASE.jar:3.2.4.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1116) ~[spring-beans-3.2.4.RELEASE.jar:3.2.4.RELEASE]
```
最重要的是这句
```
org.springframework.beans.factory.BeanCreationException:
Error creating bean with name'pmTranLimitLiteServiceImpl'
```
出现的原因：
缺少相关的jar包或者依赖

建议不要自己配置idea的module和artificts

直接在pom.xml文件添加
```
<artifactId>hisen-project</artifactId><!--加在这句话后面-->
  <packaging>war</packaging><!--加上这句话就会自动给你打war包-->
```
---

其他原因

开始不知道什么问题，后来搜索这个服务。

发现这跟dubbo有关，于是百度搜索进了官网

没想到常见问题里面就有说这个事情
```
13. 出现org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'xxxService': Initialization of bean failed; nested exception is java.lang.IllegalArgumentException: Method must not be null怎么办？

通常是classpath下存在spring多个版本的jar包，排除掉不需要的spring包即可。
```
更多dubbo问题：<a href="http://dubbo.io/FAQ-zh.htm" target="_blank">点击查看</a>