---
title: hexo百度抓取失败解决办法
keywords: [hexo,hexo百度,hexo seo]
tags: [hexo]
date: 2017-02-16 10:12:05
categories: 软件
---

上一篇帖子说明了一下百度抓取不到的原因是因为github把百度爬虫给屏蔽了

这里给出的解决办法是用hexo自动提交插件

需要获取一个自动提交的token

1. 注册百度站长工具：http://zhanzhang.baidu.com
2. 添加你的hexo域名
3. 找到网页抓取 - 链接提交 - 下拉选择你的hexo站点
4. 数据提交方式 - 自动提交 - 主动推送(实时)
5. 推送接口
6. 接口调用地址： http://data.zz.baidu.com/urls?site=hisen.me&token=XmxXyESxyz1hANxE
7. 复制上面token=后面的内容，那就是你的token

安装自动提交插件：
<!--more-->
1. npm install hexo-baidu-url-submit --save
2. 编辑站点配置文件_config.yml，添加一下内容
```
baidu_url_submit:
  count: 1 ## 提交最新的一个链接
  host: www.hui-wang.info ## 在百度站长平台中注册的域名
  token: your_token ## 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt ## 文本文档的地址， 新链接会保存在此文本文档里
```
加入新的deployer: 原来type前面是没有 - 的

但是不这样处理执行hexo g 会报错
```
deploy:
- type: git
  repository: yoururl
  branch: master
- type: baidu_url_submitter
```
生成效果如下：
```
$ hexo g
INFO  Start processing
INFO  Generating Baidu urls for last 1 posts
INFO  Posts urls generated in baidu_urls.txt
http://hisen.me/20170216-hexo百度抓取失败解决办法/
INFO  Files loaded in 1.31 s

$ hexo d
INFO  Deploy done: git
INFO  Deploying: baidu_url_submitter
INFO  Submitting urls
http://hisen.me/20170216-hexo百度抓取失败解决办法/
{"remain":1,"success":1}
INFO  Deploy done: baidu_url_submitter
```

感谢插件作者：<a href="http://hui-wang.info/2016/10/23/Hexo%E6%8F%92%E4%BB%B6%E4%B9%8B%E7%99%BE%E5%BA%A6%E4%B8%BB%E5%8A%A8%E6%8F%90%E4%BA%A4%E9%93%BE%E6%8E%A5/" target="_blank">王辉的博客</a>
