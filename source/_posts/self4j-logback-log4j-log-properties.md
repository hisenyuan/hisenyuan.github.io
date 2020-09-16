---
title: Java 日志配置文件 - self4j 之 logback 和 log4j
keywords: [java,java日志,self4j,logback,log4j,logback.xml,log4j.xml]
date: 2020/04/06 21:50
tags: [java]
categories: java
---
写这种博客总感觉很低级
但是不得不承认自己从来没有认真的了解过打日志这件事
这里开个头，先从常见的日志配置文件开始，后续再深入了解

"I wish i had" vs "I'm glad i did"
人生会不会留下遗憾，在 20 年之后的这两句话中就能看出。
希望 20 年后的自己，能够很自豪的说：I'm glad i did.

# 一、self4j
这是一个只定义了标准的日志组件
# 二、logback
常见配置：logback.xml
<!--more-->
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <property name="APP" value="hisenyuan-log-project"/>
    <!--文件输出-->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!--存储路径-->
        <file>/export/log/${APP}/${APP}_detail.log</file>
        <!--按天分割-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>/www/log/${APP}/${APP}_detail.%d{yyyy-MM-dd}.log</FileNamePattern>
        </rollingPolicy>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d{yy-MM-dd.HH:mm:ss.SSS} [%-16t] %-5p %-22c{0} %X{trace-id} - %m%n</pattern>
        </encoder>
    </appender>
    <!--控制台-->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d{yy-MM-dd.HH:mm:ss.SSS} [%-16t] %-5p %-22c{0} %X{trace-id} - %m%n</pattern>
        </encoder>
    </appender>

    <!--你想排除xxx包下面的日志-->
    <logger name="kafka.utils.Logging" additivity="false">
        <appender-ref ref="FILE"/>
    </logger>
    <!--允许WARN以及以上的打印-->
    <logger name="org.apache.http" additivity="false">
        <level value="WARN" />
        <appender-ref ref="FILE"/>
    </logger>

    <!--日志级别-->
    <root level="INFO">
        <!--启用日志文件打印日志-->
        <appender-ref ref="FILE"/>
        <!---->
        <appender-ref ref="CONSOLE"/>
    </root>
</configuration>
```
# 三、log4j
常见配置：log4j.xml
```xml
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE log4j:configuration SYSTEM
        "http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/xml/doc-files/log4j.dtd">
<log4j:configuration>
    <appender name="CONSOLE" class="org.apache.log4j.ConsoleAppender">
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d{yy-MM-dd.HH:mm:ss.SSS} [%-16t] %-5p %-22c{1} - %m%n"/>
        </layout>
        <filter class="org.apache.log4j.varia.LevelRangeFilter">
            <param name="LevelMin" value="INFO"/>
            <param name="LevelMax" value="ERROR"/>
        </filter>
    </appender>

    <appender name="FILE" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="DatePattern" value="'.'yyyy-MM-dd"/>
        <param name="Append" value="true"/>
        <param name="file" value="/www/log/hisenyuan-log-project.log"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d{yy-MM-dd.HH:mm:ss.SSS} [%-16t] %-5p %-22c{1} - %m%n"/>
        </layout>
        <filter class="org.apache.log4j.varia.LevelRangeFilter">
            <param name="LevelMin" value="INFO"/>
            <param name="LevelMax" value="ERROR"/>
        </filter>
    </appender>

    <logger name="kafka.utils.Logging" additivity="false">
        <appender-ref ref="FILE"/>
    </logger>

    <logger name="org.apache.http" additivity="false">
        <appender-ref ref="FILE"/>
    </logger>

    <root>
        <priority value="INFO"/>
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>

</log4j:configuration>
```
