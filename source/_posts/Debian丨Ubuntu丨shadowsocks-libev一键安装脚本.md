---
title: Debian丨Ubuntu丨shadowsocks-libev一键安装脚本
keywords: [shadowsocks,一键安装shadowsocks]
date: 2017/9/4 16:13
tags: [network]
categories: 工具
---
执行命令：(装完就开机启动)
```
wget --no-check-certificate -O shadowsocks-libev-debian.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev-debian.sh
chmod +x shadowsocks-libev-debian.sh
./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log
```
默认设置：
```
服务器端口：自己设定（如不设定，默认为 8989）
密码：自己设定（如不设定，默认为 teddysun.com）
加密方式：自己设定（如不设定，默认为 aes-256-gcm）
```
管理命令：
```
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
查看状态：/etc/init.d/shadowsocks status
```
修改配置文件(端口、密码)
```
vi /etc/shadowsocks-libe/config.json
{
    "server":"0.0.0.0",
    "server_port":9090,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":600,
    "method":"aes-256-gcm"
}

```
卸载命令
```
./shadowsocks-libev-debian.sh uninstall
```