---
title: Mybatis-Generator自动生成dao、model、mapper
keywords: [mybatis,Mybatis-Generator自动生成dao、model、mapper,mybatis自动生成,generatorConfig.xml配置]
date: 2017/8/2 15:35
tags: [mybatis,sql]
categories: java
---
一、配置插件
--
在resources文件夹下新建：generatorConfig.xml

内容如下：注意修改包名等信息
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<!--
详细说明请看：http://blog.csdn.net/tiantangpw/article/details/51690534
-->
<generatorConfiguration>

  <context id="mysqlgenerator" targetRuntime="MyBatis3">
    <!--数据库配置-->
    <jdbcConnection connectionURL="jdbc:mysql://127.0.0.1:3306/booksystem"
      driverClass="com.mysql.jdbc.Driver"
      password="hisen"
      userId="root"/>
      
    <!--生成Model(实体类，与数据库字段对应的bean)类存放位置-->
    <javaModelGenerator targetPackage="com.hisen.entity" targetProject="src/main/java">
      <property name="enableSubPackages" value="true"/>
      <property name="trimStrings" value="true"/>
    </javaModelGenerator>
    <!--生成映射(xxxmapper.xml)文件存放位置-->
    <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
      <property name="enableSubPackages" value="true"/>
    </sqlMapGenerator>
    <!--生成Dao类存放位置-->
    <javaClientGenerator type="XMLMAPPER" targetPackage="com.hisen.dao"
      targetProject="src/main/java">
      <property name="enableSubPackages" value="true"/>
    </javaClientGenerator>
    
    <!--要生成的表-->
    <table tableName="appointment"/>
    <table tableName="user"/>
  </context>

</generatorConfiguration>
```
二、添加maven插件
--
<!--more-->
在pom.xml中添加如下内容：plugins节点内
```
<build>
    <plugins>
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
    </plugins>
  </build>
```
三、使用
--
在idea调出mavenProject界面，选择plugins，找到mybatis-generator，双击即可