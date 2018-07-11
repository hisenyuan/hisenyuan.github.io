---
title: Ubuntu安装nginx
keywords: [Ubuntu安装nginx]
date: 2017/3/21 19:28
tags: [nginx]
categories: 软件
---
Nginx ("engine x") 是一个高性能的 HTTP 和 反向代理 服务器，也是一个 IMAP/POP3/SMTP 代理服务器。
 
Nginx 是由 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的，
 
第一个公开版本0.1.0发布于2004年10月4日。其将源代码以类BSD许可证的形式发布，
 
因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。

说明：这只是一个初步的安装，后续进一步实践

安装Nginx依赖库
---
```
#安装gcc g++的依赖库
sudo apt-get install build-essential
sudo apt-get install libtool
#安装 pcre依赖库
sudo apt-get update
sudo apt-get install libpcre3 libpcre3-dev
#安装 zlib依赖库（http://www.zlib.net）
sudo apt-get install zlib1g-dev
#安装 ssl依赖库
sudo apt-get install openssl
#下载最新版本：
wget http://nginx.org/download/nginx-1.9.9.tar.gz
#解压
tar -zxvf nginx-1.9.9.tar.gz
#进入解压目录：
cd nginx-1.9.9
#配置：
sudo ./configure --prefix=/usr/local/nginx 
#编辑nginx：
sudo make
#注意：这里可能会报错，提示“pcre.h No such file or directory”,具体详见：http://stackoverflow.com/questions/22555561/error-building-fatal-error-pcre-h-no-such-file-or-directory
#需要安装 libpcre3-dev,命令为：sudo apt-get install libpcre3-dev
#安装nginx：
sudo make install
#启动nginx：
sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
#注意：-c 指定配置文件的路径，不加的话，nginx会自动加载默认路径的配置文件，可以通过 -h查看帮助命令。
#查看nginx进程：
ps -ef|grep nginx
```
Nginx常用命令
---
<!--more-->
```
#启动
sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
#进入目录
cd /usr/local/nginx/
#关闭
sudo ./sbin/nginx -s quit
#Nginx重新加载配置
sudo ./sbin/nginx -s reload
#指定配置文件
sudo ./sbin/nginx -c /usr/local/nginx/conf/nginx.conf
#查看版本
sudo ./sbin/nginx -v
#显示帮助
sudo ./sbin/nginx -h
```
安装之后就直接监听80端口，浏览器打开127.0.0.1即可访问出现如下页面：
<img src="http://wx4.sinaimg.cn/mw690/b2e389b6ly1fdup21igsjj20g007ijrq.jpg" />