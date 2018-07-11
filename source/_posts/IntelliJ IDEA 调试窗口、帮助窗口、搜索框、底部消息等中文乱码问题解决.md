---
title: IntelliJ IDEA 调试窗口、帮助窗口、搜索框、底部消息等中文乱码问题解决
keywords: [IntelliJ IDEA 调试窗口、帮助窗口、搜索框、底部消息等中文乱码问题解决,idea底部乱码]
date: 2017/8/25 11:04
tags: [乱码]
categories: idea
---
IntelliJ IDEA 调试窗口、帮助窗口、搜索框、底部消息等中文乱码

在使用的过程中发现，搜索框历史、提交svn后的消息提示乱码

**最后发现是由于更改了idea界面的字体，字体对中文支持不佳导致**

解决办法：更改为支持中文的字体（比如：微软雅黑 Microsoft YaHei）

设置路径：Settings -> Appearance & Behavior -> UI Options -> override default fonts by

选择何时的字体即可，也可以把勾勾去掉，使用默认的。
