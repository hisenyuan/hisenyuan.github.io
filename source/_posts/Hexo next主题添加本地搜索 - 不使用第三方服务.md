---
title: Hexo next主题添加本地搜索 - 不使用第三方服务
keywords: [hexo本地搜索,hexo next主题本地搜索]
date: 2017/4/7 11:02
tags: [hexo]
categories: hexo
---

之前安装过第三方的搜索服务，贼蛋疼。都不免费了。

也有自己安装插件，然后写js的，麻烦

后来找到两个插件，安装之后就搞定了

感谢开发的作者！！！

安装插件
---

记得要在站点根目录执行下面的安装操作

1.安装 hexo-generator-search
```
npm install hexo-generator-searchdb --save
```
2.安装 hexo-generator-searchdb
```
npm install hexo-generator-searchdb --save
```
<!--more-->
启用搜索
---
编辑站点文件**_config.yml**，添加以下内容开启搜索
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
编辑主题文件**_config.yml**，启用本地搜索功能：
```
# Local search
local_search:
  enable: true
```  
效果预览
---
<img src="http://wx2.sinaimg.cn/mw690/b2e389b6ly1fedxu45zqcj20p10i4409.jpg" />

小缺点
---
第一次点击搜索的时候反应会比较慢

因为是要加载一个xml文件