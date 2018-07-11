---
title: log4j日志信息插入mysql数据库
keywords: [log4j,mysql]
date: 2017/3/21 18:41
tags: [java]
categories: java
---
以前不知道这玩意还能直接插入MySQL数据库

我是使用IDEA，maven

具体的目录结果见github：<a href="https://github.com/hisen-yuan/IDEAPractice/tree/master/src/main/java/com/hisen/log4j/log4j2MySQL" target="_blank">github</a>

log4j配置文件
---
<!--more-->
```
#可以设置级别：debug>info>error
#debug：显示debug、info、error
#info：显示info、error
#error：只error
log4j.rootLogger=debug,info,database
#注意的地方database 对应 log4j.appender.database.URL的database 若认log4j.rootLogger=debug,info,db 那么 log4j.appender.database.URL的database 要改成db
#log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender
#log4j.appender.logfile.DatePattern=.yyyy-MM-dd
#log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
#输出到控制台
#log4j.appender.appender1=org.apache.log4j.ConsoleAppender
#样式为TTCCLayout
#log4j.appender.appender1.layout=org.apache.log4j.TTCCLayout
#设置级别：
#log4j.rootLogger=debug,appender1

#输出到文件(这里默认为追加方式)
#log4j.appender.appender1=org.apache.log4j.FileAppender
#设置文件输出路径
#【1】文本文件
#log4j.appender.appender1.File=c:/Log4JDemo02.log
#【2】HTML文件
log4j.appender.appender1.File=c:/Log4JDemo02.html   
#设置文件输出样式
#log4j.appender.appender1.layout=org.apache.log4j.TTCCLayout
log4j.appender.appender1.layout=org.apache.log4j.HTMLLayout  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %p [%c] - - <%m>%n
log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender
log4j.appender.logfile.DatePattern=.yyyy-MM-dd
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] wang- <%m>%n
#下面是配置将日志信息插入数据库，
#配置输出目标为数据库（假如要将日志在控制台输出，配置为log4j.appender. stdout =org.apache.log4j.ConsoleAppender；将日志写入文件，配置为log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender
#这样的配置在许多地方都要有，需要可查有关资料）,当然你也可以自己扩展org.apache.log4j.jdbc.JDBCAppender这个类，只需要在这里配置就可以了例如我们配置我自己扩展的MyJDBCAppender，配置为#log4j.appender.db=com.neam.commons.MyJDBCAppender
log4j.appender.database.Threshold=info
#定义什么级别的错误将写入到数据库中
log4j.appender.database.BufferSize=1
#设置缓存大小，就是当有1条日志信息是才往数据库插一次
log4j.appender.database=org.apache.log4j.jdbc.JDBCAppender
log4j.appender.database.driver=com.mysql.jdbc.Driver
#设置要将日志插入到数据库的驱动
log4j.appender.database.URL=jdbc:mysql://127.0.0.1:3306/log4j?useUnicode=true&characterEncoding=UTF-8
log4j.appender.database.user=root
log4j.appender.database.password=hisen

log4j.appender.database.sql=insert into log (Class,Mothod,createTime,LogLevel,MSG) values ('%C','%M','%d{yyyy-MM-dd HH:mm:ss}','%p','%m')
log4j.appender.database.layout=org.apache.log4j.PatternLayout
```

测试类
---
```
package com.hisen.log4j.log4j2MySQL;

import org.apache.log4j.Logger;

/**
 * 测试一下log4j把日志插入到mysql数据库
 * 插入语句和数据库配置在log4j的配置文件中
 * Created by hisenyuan on 2017/3/21 at 14:23.
 */
public class Log4jDemo {
    private static Logger logger = Logger.getLogger(Log4jDemo.class);

    public static void main(String[] args) {
        logger.debug("这是debug信息");
        // 记录info级别的信息
        logger.info("这是info信息");
        logger.info("这里做了一个XX操作，入库，做操作日志");
        // 记录error级别的信息
        logger.error("这是error信息");
        hisen();
    }

    public static void hisen() {
        logger.debug("这是方法中debug信息");
        // 记录info级别的信息
        logger.info("这是方法中info信息");
        // 记录error级别的信息
        logger.error("这是方法中error信息");
    }
}
```