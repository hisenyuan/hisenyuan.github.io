---
title: Docker 入门实践
keywords: [Docker]
date: 2017/9/29 9:56
tags: [docker]
categories: docker
---
早些天不忙的时候看的入门，从有道云笔记搬过来的

# 简介
Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。
Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## Docker的应用场景
1. Web 应用的自动化打包和发布。
2. 自动化测试和持续集成、发布。
3. 在服务型环境中部署和调整数据库或其他的后台应用。
4. 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。
<!--more-->
## Docker的优点
1. **简化程序**：Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的	任务，在Docker容器的处理下，只需要数秒就能完成。
2. **避免选择恐惧症**：如果你有选择恐惧症，还是资深患者。Docker 帮你	打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。
3. **节省开支**：一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。
## Docker 架构

| 中文 | 英文 | 解释 |
|:-----|:-----|:-----|
| 镜像|Docker Images | Docker 镜像是用于创建 Docker 容器的模板。|
|容器|Docker Container|容器是独立运行的一个或一组应用。|
|客户端|Docker Client|Docker 客户端通过命令行或者其他工具使用 Docker API ([查看API](https://docs.docker.com/reference/api/docker_remote_api)) 与 Docker 的守护进程通信。|
|主机|Docker Host|一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。|
|仓库|Docker Registry|Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。Docker Hub([查看镜像](https://hub.docker.com)) 提供了庞大的镜像集合供使用。|
|工具|Docker Machine|Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。|

# Ubuntu Docker 安装
## 使用脚本安装
### 下载最新的安装包
```
hisen@hisen-pc:~$ wget -qO- https://get.docker.com/ | sh
```
### hello-world
```
#启动
sudo service docker start

#运行hello-world
docker run hello-world
##报错
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.31/containers/create: dial unix /var/run/docker.sock: connect: permission denied.
##解决:su 以root方式运行

#docker run hello-world
##提示:没有image,然后自动下载
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b04784fba78d: Pull complete 
Digest: sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

#已经启动成功
```
# Docker 使用
```
#启动某个指定的镜像,运行hello-world
root@hisen-pc:/home/hisen# docker run ubuntu:15.10 /bin/echo "Hello world"
Unable to find image 'ubuntu:15.10' locally
15.10: Pulling from library/ubuntu
7dcf5a444392: Downloading   6.29MB/51.07MB
759aa75f3cee: Download complete 
3fa871dc8a2b: Download complete 
224c42ae46e7: Download complete 

```
## 各个参数解析
docker run ubuntu:15.10 /bin/echo "Hello world"


|参数|解释|
|:-----|:-----|
|docker|Docker 的二进制执行文件|
|run|与前面的 docker 组合来运行一个容器|
|ubuntu:15.10|指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。|
|/bin/echo "Hello world"|在启动的容器里执行的命令|

以上命令完整的意思可以解释为：Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。

## 运行交互式的容器
```
root@hisen-pc:/home/hisen# docker run -i -t ubuntu:15.10 /bin/bash
##这时,我们已经进入了ubuntu:15.10的系统
##这时就是进入了命令行,可以执行相关命令
root@6265d70f44d2:/# 
##
运行exit命令或者使用CTRL+D来退出容器
```
-t:在新容器内指定一个伪终端或终端。
-i:允许你对容器内的标准输入 (STDIN) 进行交互。

## 启动容器（后台模式）
```
root@hisen-pc:/home/hisen# docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
8c2a843273608eb155279ce4e4f9f4c5442c2af3524ba50ca2d6c8ccbd207081
```
后台启动完成,会返回一串容器的ID
```
##查看是否有容器启动
root@hisen-pc:/home/hisen# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS               NAMES
8c2a84327360        ubuntu:15.10        "/bin/sh -c 'while..."   About a minute ago   Up About a minute 
```
CONTAINER ID:容器ID(这里是:==8c2a84327360==)

NAMES:自动分配的容器名称

```
## 查看上面那个ID的容器的log
root@hisen-pc:/home/hisen# docker logs 8c2a84327360
## 输出内容
hello world
hello world

## 停止容器(指定ID:8c2a84327360)
root@hisen-pc:/home/hisen# docker stop 8c2a84327360
## 输出ID,说明已经停止成功
8c2a84327360

## 检查是否真的关闭
root@hisen-pc:/home/hisen# docker ps
## 输出没有,说明真的关闭了
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

```
## Docker 客户端
docker 客户端非常简单 ,我们可以直接输入 docker 命令来查看到 Docker 客户端的所有命令选项。
```
## 调出帮助
root@hisen-pc:/home/hisen# docker
##　输出帮助信息
Usage:	docker COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/root/.docker")
  -D, --debug              Enable debug mode
      --help               Print usage
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level

## 了解更详细的帮助信息
root@hisen-pc:/home/hisen#docker　stats --help

## 输出详细帮助信息
Usage:	docker stats [OPTIONS] [CONTAINER...]

Display a live stream of container(s) resource usage statistics

Options:
  -a, --all             Show all containers (default shows just running)
      --format string   Pretty-print images using a Go template
      --help            Print usage
      --no-stream       Disable streaming stats and only pull the first result

```
## 运行一个WEB容器
运行一个 Python Flask 应用 来搭建一个web应用
```
## -d:让容器在后台运行。
## -P:将容器内部使用的网络端口映射到我们使用的主机上。
root@hisen-pc:/home/hisen# docker run -d -P training/webapp python app.py
## 提示本地没有,远程下载
Unable to find image 'training/webapp:latest' locally
latest: Pulling from training/webapp
e190868d63f8: Pull complete 
909cd34c6fd7: Pull complete 
0b9bfabab7c1: Pull complete 
a3ed95caeb02: Pull complete 
10bbbc0fc0ff: Pull complete 
fca59b508e9f: Pull complete 
e7ae2541b15b: Pull complete 
9dd97ef58ce9: Pull complete 
a4c1b0cb7af7: Pull complete 
Digest: sha256:06e9c1983bd6d5db5fba376ccd63bfa529e8d02f23d5079b8f74a616308fb11d
Status: Downloaded newer image for training/webapp:latest

## 下载完成直接后台运行了,打印id
b03903d6abf3d65a4e6469a57d11ae539aaad1f166078591e7315ca2fb2611ff

## 查看运行的docker
root@hisen-pc:/home/hisen# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
b03903d6abf3        training/webapp     "python app.py"     9 minutes ago       Up 9 minutes        0.0.0.0:32768->5000/tcp   nifty_mirzakhani
```
0.0.0.0:32768->5000/tcp  
docker使用的端口5000

映射到本地端口32768

本地可以访问:[127.0.0.1:32768](127.0.0.1:32768)
```
## 访问:http://localhost:32768/
## 输出
Hello world!

## 可以指定端口 8060为自映射端口,5000为容器端口
root@hisen-pc:/home/hisen# docker run -d -p 8060:5000 training/webapp python app.py
0d68d3d4baf61248129ede721cc6004649ebddaaace8c23f16b677fbe2b2ce7d
root@hisen-pc:/home/hisen# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
0d68d3d4baf6        training/webapp     "python app.py"     5 seconds ago       Up 4 seconds        0.0.0.0:8060->5000/tcp   ecstatic_jang

## 查看端口映射情况(可以通过id,或者名字)
## 这里用ID测试
root@hisen-pc:/home/hisen# docker port 0d68d3d4baf6
## 输出信息,后面为映射到本地的端口
5000/tcp -> 0.0.0.0:8060
```
### 查看WEB应用程序日志
```
## 带上 -f 参数,动态输出日志
root@hisen-pc:/home/hisen# docker logs -f 0d68d3d4baf6
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)

## 刷新页面之后的日志
172.17.0.1 - - [01/Sep/2017 14:09:01] "GET / HTTP/1.1" 200 -
172.17.0.1 - - [01/Sep/2017 14:09:01] "GET /favicon.ico HTTP/1.1" 404 -
```
### 查看WEB应用程序容器的进程
```
## top后面是跟着id或者name
root@hisen-pc:/home/hisen# docker top 0d68d3d4baf6
## 输出的信息
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                19372               19352               0                   21:39               ?                   00:00:00            python app.py
```
### 检查WEB应用程序
```
## 会输出一大串JSON形式的字符串
## 记录着 Docker 容器的配置和状态信息
root@hisen-pc:/home/hisen# docker inspect 0d68d3d4baf6
```
### WEB程序其他操作
```
docker stop  0d68d3d4baf6
## 移除之前必须停止,否则会报错
docker rm 0d68d3d4baf6
```

# Docker 镜像使用
## 列出所有本地的镜像
```
root@hisen-pc:/home/hisen# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
hello-world          latest              1815c82652c0        2 months ago        1.84kB
woailuoli993/jblse   0.2.0               82099e8d7049        6 months ago        7.58MB
ubuntu               15.10               9b9cb95443b5        13 months ago       137MB
training/webapp      latest              6fae60ef3446        2 years ago         349MB
```
## 其他命令
```
## 获取一个镜像
## ubuntu:镜像库中的TAG
## 13.10:某个TAG的版本
docker pull ubuntu:13.10


## 搜索某个镜像
docker search httpd

## 拖取镜像
docker pull httpd

## 运行镜像
docker run httpd

## 创建镜像
### 进入某个镜像的bash命令行(在此镜像基础上创建自己的镜像)
root@hisen-pc:/home/hisen# docker run -t -i ubuntu:15.10
### 更新系统(镜像的)
root@4a3f5f4373d8:/# apt-get update

##提交容器副本
### 退出镜像的bash命令界面
root@4a3f5f4373d8:/# exit          
exit
### 提交副本
root@hisen-pc:/home/hisen# docker commit -m="has update" -a="hisen" 4a3f5f4373d8 hisen/ubuntu:v2
### 返回副本容器的id
sha256:a94d0bdd2e31aa420c14e9886ff911354c07a7772715bd8d380bd0832f396c82
### 查看,发现刚刚提交的副本显示出来了
root@hisen-pc:/home/hisen# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED              SIZE
hisen/ubuntu         v2                  a94d0bdd2e31        About a minute ago   159MB
hello-world          latest              1815c82652c0        2 months ago         1.84kB
woailuoli993/jblse   0.2.0               82099e8d7049        6 months ago         7.58MB
ubuntu               15.10               9b9cb95443b5        13 months ago        137MB
training/webapp      latest              6fae60ef3446        2 years ago          349MB

## 参数说明
-m:提交的描述信息
-a:指定镜像作者
4a3f5f4373d8：容器ID(就是你修改之前的id)
hisen/ubuntu:v2:指定要创建的目标镜像名

## 进入新镜像的bash命令
docker run -t -i hisen/ubuntu:v2 /bin/bash 
```
## 构建一个全新的镜像
新建一个配置文件:Dockerfile,并添加内容

每个指令都会在镜像上创建一个新的层

每一个指令的前缀都必须是大写的。

第一条FROM，指定使用哪个镜像源

RUN 指令告诉docker 在镜像内执行命令，安装了什么
```
root@hisen-pc:/home/hisen# vi Dockerfile 

FROM    ubuntu:16.04
MAINTAINER      Fisher "hisenyuan@gmail.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd hisen
RUN     /bin/echo 'hisen:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D

## 保存配置文件之后,构建镜像,注意最后那个点.
root@hisen-pc:/home/hisen# docker build -t hisen/ubuntu:16.04 .
sending build context to Docker daemon    776MB
```