---
title: ubuntu安装docker-ce并配置国内源和加速器
keywords: [ubuntu安装docker,docker国内镜像]
date: 2017/4/17 15:22
tags: [ubuntu,docker]
categories: linux
---
一、配置ubuntu国内镜像，这里推荐阿里云，右上角搜索：换阿里云源
---

二、安装docker
---
```
sudo apt-get update
sudo apt-get install \
     linux-image-extra-$(uname -r) \
     linux-image-extra-virtual
```
安装docker包
```
sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     software-properties-common 
```
添加docker官方GPG秘钥,留意最后那个符号也要复制
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
安装稳定版仓库
<!--more-->
```
sudo add-apt-repository \
     "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) \
     stable"
```
再次更新源
```
sudo apt-get update
```
安装docker-ce
```
sudo apt-get install docker-ce
```

三、给docker添加国内加速器
---
在阿里云申请一个账号，打开连接<a href="https://cr.console.aliyun.com/#/accelerator" target="_blank">https://cr.console.aliyun.com/#/accelerator</a> 

拷贝您的专属加速器地址（每个人专属的，登陆需要密码），然后
```
vi /etc/systemd/system/multi-user.target.wants/docker.service
```
可以看到如下内容
```
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
#下面这行是默认的，我注释了，添加了下面一行
#ExecStart=/usr/bin/dockerd -H fd://
ExecStart=/usr/bin/dockerd -H fd:// --registry-mirror=https://9s3ekxxx.mirror.aliyuncs.com
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=1048576
```
找到 ExecStart= 这一行，在这行最后添加加速器地址 --registry-mirror=<加速器地址>

如：ExecStart=/usr/bin/dockerd -H fd://  --registry-mirror=https://xxxxxx.mirror.aliyuncs.com

四、重新加载配置并且重新启动
---
```
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```
至此docker安装及国内加速器都好了，开始你的docker之旅吧。
```
sudo docker run hello-world
```
看到如下信息
```
hisen@ubuntu:/$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
78445dd45222: Pull complete 
Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```
到此就圆满结束

最后给个彩蛋，阿里云一键安装脚本，执行下面命令即可安装最新版docker
```
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
```
详情：<a href="http://mirrors.aliyun.com/help/docker-engine" target="_blank">http://mirrors.aliyun.com/help/docker-engine</a>