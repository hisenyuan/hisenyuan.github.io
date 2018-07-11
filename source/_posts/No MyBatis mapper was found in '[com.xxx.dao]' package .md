---
title: No MyBatis mapper was found in 'com.xxx.dao' package 
keywords: [No MyBatis mapper was found in,No MyBatis mapper was found in 'com.xxx.dao' package]
date: 2017/8/1 15:26
tags: [java,xml]
categories: java
---

出错
---
maven项目，启动tomcat的时候报错：
```
No MyBatis mapper was found in '[com.hisen.dao]' package 
```
这是由于把mybatis的mapper配置文件放在了java代码的目录下

目录结构
---
```
├── java
│   └── com
│       └── hisen
│           ├── dao
│           │   └── BookDao.java
│           │   └── BookMapper.xml
│           ├── entity
│           │   └── Book.java
│           ├── service
│           │   ├── BookService.java
│           │   └── impl
│           │       └── BookServiceImpl.java
│           └── web
│               └── BookController.java
├── resources
│   ├── jdbc.properties
│   ├── logback.xml
│   ├── mybatis-config.xml
│   └── spring
│       ├── spring-dao.xml
│       ├── spring-service.xml
│       └── spring-web.xml
└── webapp
    ├── index.jsp
    └── WEB-INF
        ├── jsp
        │   ├── detail.jsp
        │   └── list.jsp
        └── web.xml
```

出错原因
---
<!--more-->
这是因为mapper文件的路径问题：maven在打包的时候默认是认为src/main/java目录下只有java源代码，不会管xml文件。

Java 项目分为两个部分，一个是源码，一个是资源，在使用maven等构建工具时，

默认会将源码编译后再加上资源目录的文件放到target目录下作为最后运行的文件（可以是war,jar,或者目录）。

有时为了方便，我们会在src/main/java源码目录下放了资源文件，例如mybatis的mapper文件，方便我们编程时展开查看。

这时，我们需要设置编译时也将这些配置文件放到target目录下，否则最后的target目录式没有这些文件的。

解决办法
---
在项目的pom文件里面配置，让maven也从src/main/java目录下读取xml配置文件
```
 <build>
        <!--注意,如果将静态资源放在src/main/java中,那么编译时将被maven忽略,
        在target目录下将没有这些资源,所以需要将该目录添加为资源目录.
        -->
        <resources>
          <resource>
            <directory>${basedir}/src/main/java</directory>
            <includes>
              <include>**/*.xml</include>
            </includes>
          </resource>
        </resources>
 </build>
```
扩展
---
如果是把mapper文件什么的放在resources目录下

但是为了分类建了不同的文件夹，也需要修改配置文件，不然会读取不到
```
<build>
  <!--包含下面所有的配置文件（包括子目录）-->
  <resources>
    <resource>
      <directory>${basedir}/src/main/resources</directory>
      <includes>
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
    </resource>
  </resources>
</build>
```