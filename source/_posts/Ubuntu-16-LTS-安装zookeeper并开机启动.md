---
title: Ubuntu 16 LTS 安装zookeeper并开机启动
keywords: [安装zookeeper并开机启动,Ubuntu安装zookeeper]
tags: [zookeeper]
date: 2017-02-24 10:03:24
categories: linux
---

# 一、安装
```
sudo apt-get install zookeeper
```
默认信息
```
#安装路径
/usr/share/zookeeper
#配置文件
/etc/zookeeper/conf/zoo.cfg
```
# 二、启动服务端
<!--more-->
```
hisen@hisen-server:/usr/share/zookeeper/bin$ sudo sh zkServer.sh
```
如果报错
```
zkServer.sh: 81: /home/xxx/zookeeper-3.4.6/bin/zkEnv.sh: Syntax error: "(" unexpected (expecting "fi")
网上找了一圈原因，大概意思就是脚本里面用到的shell版本与系统当前使用的shell版本不兼容，导致异常。
查看当前ubuntu系统的shell，默认是使用dash，但是脚本里面是使用的bash，问题就在这里了。
解决步骤：
修改当前系统的shell版本：
dpkg-reconfigure dash
Tab 移动到NO 回车即可(选择否)
```
# 三、验证是否成功
```
hisen@hisen-server:/usr/share/zookeeper/bin$ sudozkCli.sh -server localhost:2181
WatchedEvent state:SyncConnected type:None path:null
[zk: localhost:2181(CONNECTED) 0]
```
出现上面的信息说明成功了
# 四、设置开机启动
1.创建配置文件
```
sudo vi /etc/init.d/zookeeper
```
添加以下信息，注意自己的相关路径是否相同，不同修改之
```
#!/bin/sh
#Configurations injected by install_server below....
EXEC=/usr/share/zookeeper/bin/zkServer.sh
ZOO_LOG_DIR="/var/zookeeper"
JAVA_HOME=/usr/hisen/soft/jdk8 
PATH=${JAVA_HOME}/bin:$PATH
###############
# SysV Init Information
# chkconfig: - 58 74
# description: zookeeper is the zookeeper daemon.
### BEGIN INIT INFO
# Provides: zookeeper
# Required-Start: $network $local_fs $remote_fs
# Required-Stop: $network $local_fs $remote_fs
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Should-Start: $syslog $named
# Should-Stop: $syslog $named
# Short-Description: start and stop zookeeper
# Description: zookeeper daemon
### END INIT INFO
case $1 in
          start)  /usr/share/zookeeper/bin/zkServer.sh start;;
          stop)   /usr/share/zookeeper/bin/zkServer.sh stop;;
          status) /usr/share/zookeeper/bin/zkServer.sh status;;
          restart) /usr/share/zookeeper/bin/zkServer.sh restart;;
          *)  echo "require start|stop|status|restart"  ;;
esac
```
2.授权
```
sudo chmod +x zookeeper
```
3.安装开机启动管理软件(一般自带)
```
sudo apt-get install rcconf
```
4.进入管理界面
```
sudo rcconf
```
↑ ↓ 移动光标，空格键选中zookeeper

Tab 使光标移动到OK 回车即可