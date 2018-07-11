---
title: Hexo Landscape主题JS优化，不使用谷歌CDN
date: 2017-02-08 22:21:52
tags: [hexo优化]
---
jQuery库的优化
---
修改这个文件：
```
themes/landscape/layout/_partial/after-footer.ejs
```
将17行左右的
```
<script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
```
改为
```
<script src="http://apps.bdimg.com/libs/jquery/2.0.3/jquery.min.js"></script>
```
重新生成博客之后就把谷歌的cdn替换成百度的cdn了