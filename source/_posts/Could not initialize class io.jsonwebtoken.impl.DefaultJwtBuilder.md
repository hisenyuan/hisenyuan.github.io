---
title: Could not initialize class io.jsonwebtoken.impl.DefaultJwtBuilder
keywords: [jsonwebtoken,jwt,token]
date: 2018/10/15 14:15
tags: [jsonwebtoken]
categories: java
---
使用jsonwebtoken出现如下错误
```
Could not initialize class io.jsonwebtoken.impl.DefaultJwtBuilder
java.lang.NoClassDefFoundError: Could not initialize class io.jsonwebtoken.impl.DefaultJwtBuilder
	at io.jsonwebtoken.Jwts.builder(Jwts.java:116) ~[jjwt-0.7.0.jar:0.7.0]
```
原因,因为jackson-databindb版本冲突，直接去掉了依赖

jwt必须依赖Jackson所以报错了

出错时候的配置
```
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.7.0</version>
            <exclusions>
                <exclusion>
                    <artifactId>jackson-databind</artifactId>
                    <groupId>com.fasterxml.jackson.core</groupId>
                </exclusion>
            </exclusions>
        </dependency>
```
解决办法
```
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.7.0</version>
        </dependency>
```
