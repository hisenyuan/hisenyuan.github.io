---
title: 笔记本在BIOS Setup里面设置双显卡模式
keywords: [笔记本设置独显,笔记本设置双显卡,笔记本显卡]
date: 2017/5/9 9:18
tags: [软件]
categories: 软件
---
我的本子是双显卡的，英特尔的核芯显卡和英伟达的独显

但是之前完全没有把独显用上，在英伟达的设置里选择使用显卡感觉也没有用

后来找到在BIOS里面设置，貌似管用，联想的机子可以看看

其他的机子应该也差不多

连接：<a href="http://iknow.lenovo.com/detail/dc_102471.html" target="_blank">http://iknow.lenovo.com/detail/dc_102471.html</a>
```
6. IdeaPad Z380/Z480/Z580/U310/U410，Lenovo G480A/V370A/V470A/V570A
依次选择“Configuration”、“Graphic Device”，其中有两个选项：Optimus表示可切换显卡模式；
UMA Only表示集显模式。选择好后，按F10并根据提示保存退出即可。
```
我的默认居然是：UMA Only