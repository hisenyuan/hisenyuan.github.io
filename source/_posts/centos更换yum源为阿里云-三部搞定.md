---
title: centos更换yum源为阿里云 - 三步搞定
keywords: [centos更换yum源为阿里云]
tags: [linux,centos]
date: 2017-02-17 17:31:19
categories: linux
---
阿里云是最近新出的一个镜像源。得益于阿里云的高速发展，这么大的需求，肯定会推出自己的镜像源。
阿里云Linux安装镜像源地址：http://mirrors.aliyun.com/

CentOS系统更换软件安装源

第一步：备份你的原镜像文件，以免出错后可以恢复。

1、备份
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

2、下载新的CentOS-Base.repo 到/etc/yum.repos.d/

（如果无wget命令，底部有具体说明）

CentOS 5
```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
```
CentOS 6
```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
```
CentOS 7
```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
3、之后运行yum makecache生成缓存

---

ps：如果你跟我一样苦逼：
```
-bash: wget: command not found
```
然后：
```
yum -y install wget #失败
```
那么你可以直接选择上面对应系统的文件下载链接

下载好文件之后改名为CentOS-Base.repo

直接放到/etc/yum.repos.d/目录下即可
