---
title: Intellij IDEA中使用MyBatis-generator 自动生成MyBatis代码
keywords: [MyBatis-generator,idea]
date: 2017/3/22 10:08
tags: [idea,mybatis]
categories: java
---
MyBatis Generator是一个非常方便的代码生成工具，

可以根据数据库中表结构自动生成CRUD代码，可以满足大部分需求。

MyBatis Generator (MBG) 是一个Mybatis的代码生成器 ，

可以根据数据库中表结构自动生成简单的CRUD（插入，查询，更新，删除）操作。
 
但联合查询和存储过程，需手动手写SQL和对象。

**PS:配置过程中请注意自己的工程目录结构**

一、pom.xml添加插件
---
```
<plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.2</version>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.5</version>
                    </dependency>
                </dependencies>
                <configuration>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
```
二、配置generatorConfig.xml
---
<!--more-->
resources下建generatorConfig.xml,作为mybatis-generator-maven-plugin插件的执行目标。
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <context id="mysqlgenerator" targetRuntime="MyBatis3">
        <!--数据库连接信息-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/ssm"
                        userId="root"
                        password="hisen" />
        <!--代码相关路径和包-->
        <javaModelGenerator targetPackage="com.hisen.mybatis.model" targetProject="src/main/java" />

        <sqlMapGenerator targetPackage="com.hisen.mybatis.mapper" targetProject="src/main/resources" />

        <javaClientGenerator type="XMLMAPPER" targetPackage="com.hisen.mybatis.mapper" targetProject="src/main/java" />
        <!--表名-->
        <table tableName="appointment"/>
        <table tableName="book"/>

    </context>

</generatorConfiguration>
```
三、Intellij配置
---
MyBatis Generator生成代码的运行方式：命令行、使用Ant、使用Maven、Java编码。

本文采用Maven插件mybatis-generator-maven-plugin来运行MyBatis Generator，用的是命令行的方式。

配置插件
<img src="http://wx1.sinaimg.cn/mw690/b2e389b6ly1fdvexcbif9j20cs0670sz.jpg" />
选择目录，输入命令：mybatis-generator:generate -e
<img src="http://wx4.sinaimg.cn/mw690/b2e389b6ly1fdvexcxg0fj20pa0kt404.jpg" />
找到插件。双击执行
<img src="http://wx3.sinaimg.cn/mw690/b2e389b6ly1fdvexdf3u2j20dc0dft9x.jpg" />
即可看到生成的文件
<img src="http://wx1.sinaimg.cn/mw690/b2e389b6ly1fdvf14o2y3j20du0bot92.jpg" />