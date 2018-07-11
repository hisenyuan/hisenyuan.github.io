---
title: mybatis - Invalid bound statement (not found)
keywords: [mybatis]
date: 2017/8/2 11:39
tags: [mybatis]
categories: java
---
运行报错
```
org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): com.hisen.dao.UserMapper.insert
```
使用mybatis生成插件，产生的mapper，由于路径不对移动了一下mapper.java文件

所以造成mapper.xml里面的namespace错误，无法映射

所以把namespace改为正确的即可

例如：

mapper.UserMapper

改为

com.hisen.dao.UserMapper
