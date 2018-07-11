---
title: IDEA 貌似注解失效 - 报错：cannot resolve method xxx
keywords: [cannot resolve method,idea cannot resolve method]
tags: [java,idea]
date: 2017-03-01 17:15:52
categories: java
---
在java平台上lombok提供了简单的注解的形式

来帮助我们消除一些必须有但看起来很臃肿的代码

比如属性的get/set，及对象的toString等方法，特别是相对于 POJO;

---

## 出现问题 ##
1. 各种log方法，set方法中红色波浪线
2. 提示：cannot resolve method xxx
3. 虽然出现错误但是编译是可以通过的

## 问题原因 ##
原来的代码用了lombok简单注解
比如maven的pom.xml文件有如下配置
```
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.16.8</version>
		</dependency>
```

## 解决办法 ##
安装**lombok plugin**

---

装完插件之后就舒服了，也不报错