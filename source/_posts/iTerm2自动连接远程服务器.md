---
title: iTerm2自动连接远程服务器
keywords: [iTerm2,ssh,自动登录远程服务]
date: 2018/07/20 01:40
categories: linux
---

mac自带的终端总感觉不大好用，于是乎就被安利了iTerm2

然后就搜了下自动连接远程服务，于是就发现了一个不错的脚步

整个设置过程还是比较顺畅，

操作步骤：
1. 新建一个sh文件
```
vi auto_ssh.sh
```
2. 输入如下内容，[lindex $argv 0] 这个为第一个参数的占位符
```shell
#!/usr/bin/expect

set timeout 30
spawn ssh -p[lindex $argv 0] [lindex $argv 1]@[lindex $argv 2]
expect {
"(yes/no)?"
{send "yes\n";exp_continue}
"password:"
{send "[lindex $argv 3]\n"}
}
interact
```
3. 复制脚本到bin下并且赋予执行权限
```
sudo cp auto_ssh.sh /usr/local/bin/
cd /usr/local/bin/
sudo chmod +x auto_ssh.sh
```
4. 设置
在iTerm2中Command+o呼出profile
```
 # 界面右下角，点击：
 edit profiles
 # 界面左下角，点击：
 +
 # 界面上方，点击：
 General
 # 界面靠上，写这个登录的名字：
 name
 # 界面中间，点击选择：
 Login shell
 # 找到这个提示符：Send text at start，输入如下字符
 auto_ssh.sh 8022 root 10.10.20.20 hisenyuan
 # 脚本名称   端口  用户名  ip地址       密码
 ```

 5. 运行
 设置完了之后，关闭窗口，重新command+o呼出配置文件，双击刚刚配置的即可自动登录
