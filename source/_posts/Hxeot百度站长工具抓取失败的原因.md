---
title: Hxeo百度站长工具抓取失败的原因
keywords: [Hxeo,Hxeo百度抓取失败]
tags: [hexo]
date: 2017-02-16 09:54:21
categories: 软件
---

近期在折腾下hexo博客

发现百度搜索网址都搜索不到自己的站点

我只把hexo传到github上，然而去百度站长工具提交网址几条后也没有反应

我就测试了一下百度站长工具 - 抓取诊断

结果是抓取失败:**Github把百度的爬虫给干掉了**！所以。。。

---

具体内容如下：
```
提交网址：	http://hisen.me/20170214-maven%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/
抓取网址：	http://hisen.me/20170214-maven%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/
抓取UA：	Mozilla/5.0 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)
抓取时间：	2017-02-16 09:48:55
网站IP：	151.***.***.133 已反馈，预计几分钟内完成更新
下载时长：	0.331秒
抓取异常信息：	拒绝访问  查看帮助 
```
返回HTTP头：
```
HTTP/1.1 403 Forbidden
Cache-Control: no-cache
Content-Type: text/html
Transfer-Encoding: chunked
Accept-Ranges: bytes
Date: Thu, 16 Feb 2017 01:48:55 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-itm7421-ITM
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1487209735.687949,VS0,VE187
Vary: Accept-Encoding
X-Fastly-Request-ID: 3e528056d3333113dfb5da5c92a2421fa71f3705
```