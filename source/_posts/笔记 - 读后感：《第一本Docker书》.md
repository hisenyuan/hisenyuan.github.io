---
title: 笔记 - 读后感：《第一本Docker书》
keywords: [docker,第一本docker书]
date: 2018/1/27 20:59
tags: [docker]
categories: docker
---
忘记开始是在什么地方看到docker这个东西

后面觉得挺好玩，也试了很多次，找过很多的教程。

搭建了一个zookeeper的集群(docker-compose)

然后当我需要搭建redis集群的时候，发现里面很多的概念还不是很懂

比如：volume network

然后加了docker的群，遇到了在以前idea群里面熟的一个人

花了一天的时间看完他传的一本书，昨天买的几本书晚上也到了。
（docker从入门到实践、java并发编程的艺术、effective java中文版 2）

这本书总体来说还行，就是过时了，还在用link，毕竟三年前的东西

有些例子也报错，主要是ruby相关的不行，提醒需要2.2以上的版本

下面是随便做的一点笔记
<!--more-->
```
# 所有容器，不加-a只显示正在运行的；-1 列出最后一次运行的
docker ps -a
# 启动一个已经停止的容器(id，--name 都可以)
docker start hisen_container
# 进入容器(容器重启之后进入交换行)
docker attach hisen_container
# 创建一个守护式容器
docker -d
# 查看日志 可选 -f 类似tail -f Ctrl+c 退出
docker logs hisen_container
# 查看容器内的进程
docker top hisen_container
# 在容器内部运行进程
docker exec -d hisen_container touch /etc/new_conf_file
# 停止守护式容器
docker stop hisen_container
# 自动重启容器
docker run --restart=always ....
=on-failure:5 当容器退出代码为非0时，docker会自动重启，最多重试5次
# 深入容器,会以json串显示更多容器信息
docker inspect hisen_container 
# 删除容器，要先stop才行
docker rm hisen_container
# 删除所有容器 -a:所有容器 -q:只返回容器的ID
docker rm 'docker ps -a -q'
# 启动新的名字为other_hisen_container的容器，并且进入bash shell
docker run -i -t --name other_hisen_container ubuntu /bin/bash
# docker文件系统层
可写容器
镜像(比如tomcat)
基础镜像(比如ubuntu)
引导文件系统(容器组、命名空间、设备映射)
内核
# docker简单构成
顶层的读写层+底部不可变的可读层还有一些配置，就成了一个容器。
# 列出所有镜像
docker images
# 仓库名:tag 如果没有指定，默认latest
# 推荐使用Dockerfile构建镜像
FROM ubuntu:14.04
MAINTAINER hisenyuan "hisenyuan@gmail.com"
RUN apt-get update \
&& apt-getinstall -y nginx \
&& echo 'Hi,I am in your container' \
   /usr/share/nginx/html/index.html
EXPOSE 80
# 构建镜像，在上面的dockerfile目录
docker build -t="hisenyuan/static_web" .
# 查看构建过程，以及镜像的每一层
docker history 791397495f7a
# 运行构建的镜像 -p端口映射 nginx -g "daemon off;" 前台运行nginx
docker run -d -p 80:80 --name static_web hisenyuan/static_web nginx -g "daemon off;"
# Dockerfile指令
RUN 镜像构建的时候才会执行
CMD 启动一个容器的时候要运行的命令，如有多个，只会生效最后一个，前面的多个会被忽略
ENTRYPOINT 例子:ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"],这个命令不容易覆盖，CMD容易被RUN覆盖
WORKDIR 设定工作目录，ENTRYPOINT CMD 会在这个目录执行
ENV 设置构建时的环境变量，设置之后可以在后续的RUN命令使用
USER 设置运行的用户
VOLUME 向基于镜像创建的容器添加卷。
1.卷可以在容器间共享和重用
2.一个容器不是必须和其他容器共享卷
3.对卷的修改是立即生效的
4.对卷的修改不会对更新镜像产生影响
5.卷会一直存在，直到没有任何容器再使用他
VOLUME ["/opt/project"] 创建一个名为/opt/project的挂载点
ADD 添加上下文目录的文件到构架容器中，如果是压缩包，会自动解压。会使构建缓存无效
COPY 类似ADD、只会在上下文复制文件、也不会解压
ONBUILD 为镜像添加触发器，当一个镜像被用作其他镜像的基础镜像时，该镜像中的触发器将会被执行。
# 删除镜像，会删除这个镜像的每一层
docker rmi hisenyuan/static_web
# 运行一个registry
docker run -p 5000:5000 registry
# 推送镜像到registry
docker tag 791397495f7a yourip:5000/hisenyuan/static_web #打标签
docker push yourip:5000/hisenyuan/static_web #推送到本地注册容器
docker run -t -i docker yourip:5000/hisenyuan/static_web /bin/bash #从registry运行一个容器

==========第五章 在测试中使用docker==========
5.1 使用docker测试静态网站
5.1.1 sample网站的初始Dockerfile
hisen@docker1:~/docker$ mkdir sample
hisen@docker1:~/docker$ cd sample/
hisen@docker1:~/docker/sample$ touch Dockerfile
hisen@docker1:~/docker/sample$ mkdir nginx && cd nginx
hisen@docker1:~/docker/sample/nginx$ vi global.conf
server {
        listen          0.0.0.0:80;
        server_name     _;

        root            /var/www/html/website;
        index           index.html index.htm;

        access_log      /var/log/nginx/default_access.log;
        error_log       /var/log/nginx/default_error.log;
}

hisen@docker1:~/docker/sample/nginx$ vi nginx.conf
user www-data;
worker_processes 4;
pid /run/nginx.pid;
daemon off;

events {  }

http {
  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;
  include /etc/nginx/mime.types;
  default_type application/octet-stream;
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  gzip on;
  gzip_disable "msie6";
  include /etc/nginx/conf.d/*.conf;
}
hisen@docker1:~/docker/sample/nginx$ vi Dockerfile
FROM ubuntu:14.04
MAINTAINER hisenyuan "hisenyuan@gmail.com"
ENV REFRESHED_AT 2018-01-27
RUN apt-get update \
 && apt-get -y -q install nginx \
 && mkdir -p /var/www/html
ADD nginx/global.conf /etc/nginx/conf.d/
ADD nginx/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
hisen@docker1:~/docker/sample/nginx/website$ vi index.html
<head>
	<title>Test website</title>
</head>
<body>
	<h1>This is a test website</h1>
</body>
hisen@docker1:~/docker/sample/nginx$ docker build -t hisenyuan/nginx .
hisen@docker1:~/docker/sample/nginx$ docker run -d -p 8080:80 --name website \
> -v $PWD/website:/var/www/html/website \
> hisenyuan/nginx nginx # 因为已经在配置文件指定了daemon off，所以能直接启动
# -v 指定了卷的源目录，挂载本地$PWD/website到容器/var/www/html/website目录，conf配置文件指定了工作目录

# 修改之后，刷新浏览器，立马就生效
hisen@docker1:~/docker/sample/nginx$ vi $PWD/website/index.html
<head>
<title>Test website - hisenyuan</title>
</head>
<body>
<h1>This is a test website</h1>
<h2>2018年1月27日 13:20:13</h2>
</body>

5.2 使用docker构建并测试web应用程序
5.2.1 构建sinatra应用程序
# 创建Dockerfile
hisen@docker1:~/docker/sinatra$ vi Dockerfile
FROM ubuntu:14.04
MAINTINER hisenyuan "hisenyuan@gmail.com"
ENV REFRESHED 2018-01-27
RUN apt-get upate \
 && apt-get -y install ruby ruby-dev build-essential redis-tools \
 && gem install --no-rdoc --no-ri sinatra json redis \
 && mkdir -p /opt/webapp
EXPOSE 4567

# 代码网站：https://dockerbook.com/code/5/sinatra/webapp/
wget --cut-dir=3 -nH -r --no-parent https://dockerbook.com/code/5/sinatra/webapp/

5.3 docker用于持续集成

==========第六章 使用docker构建服务==========
6.1 构建第一个应用
==========第七章 Consul、服务发现与注册==========
```