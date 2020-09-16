---
title: Hexo提速优化 - 压缩html、css、js by hexo-neat插件
tags: [hexo]
date: 2017-02-11 22:41:10
keywords: [hexo优化,hexo提速,hexo压缩]
categories: 软件
---

hexo博客生成的HTML代码留有大量的空白

通过搜索发现有不错的方法可以解决这个问题

那就是安装一个插件！名字叫：hexo-neat

安装
---
命令行进入博客根目录，执行以下命令
```
npm install hexo-neat --save
```
如果使用的是淘宝的cnmp执行以下命令
```
cnpm install hexo-neat --save
```
配置
---

打开站点配置文件**_config.yml**，添加以下内容：
<!--more-->

```
#hexo-neat 优化提速插件
neat_enable: true

neat_html:
  enable: true
  exclude:

neat_css:
  enable: true
  exclude:
    - '*.min.css'

neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '*.min.js'
```

安装好之后，就直接正常使用(跟以前没有装的时候一样使用)，

重新生成博客就可以发现html源码已经压缩了

只是在生成博客的过程中可能要浪费一丁点时间

访问速度有所提升！

参考：<a href="https://segmentfault.com/a/1190000005799759" target="_blank">点击查看<a>
