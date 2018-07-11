---
title: 查看linux进程 - 多重方式
keywords: [linux查看进程,linux进程查看]
tags: [linux]
date: 2017-02-27 09:14:02
categories: linux
---
可以使用ps命令。
它能显示当前运行中进程的相关信息，包括进程的PID。

Linux和UNIX都支持ps命令，显示所有运行中进程的相关信息。

ps命令能提供一份当前进程的**快照**。如果想状态可以自动刷新，可以使用top命令。

## ps命令 ##

输入下面的ps命令，显示所有运行中的进程：
```
ps aux | less
```
**这个命令按  q  退出**
后面加了“**| less**”就会分页显示，如果去掉会一次性显示出所有结果

输出：
<!--more-->
```
hisen@hisen-server:~$ ps aux | less
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.2  0.5  37956  6028 ?        Ss   09:03   0:02 /sbin/init
root         2  0.0  0.0      0     0 ?        S    09:03   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    09:03   0:00 [ksoftirqd/0]
root         4  0.0  0.0      0     0 ?        S    09:03   0:00 [kworker/0:0]
root         5  0.0  0.0      0     0 ?        S<   09:03   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S    09:03   0:00 [kworker/u2:0]
root         7  0.0  0.0      0     0 ?        S    09:03   0:00 [rcu_sched]
root         8  0.0  0.0      0     0 ?        S    09:03   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    09:03   0:00 [migration/0]
root        10  0.0  0.0      0     0 ?        S    09:03   0:00 [watchdog/0]
root        11  0.0  0.0      0     0 ?        S    09:03   0:00 [kdevtmpfs]
root        12  0.0  0.0      0     0 ?        S<   09:03   0:00 [netns]
root        13  0.0  0.0      0     0 ?        S<   09:03   0:00 [perf]
root        14  0.0  0.0      0     0 ?        S    09:03   0:00 [khungtaskd]
root        15  0.0  0.0      0     0 ?        S<   09:03   0:00 [writeback]
root        16  0.0  0.0      0     0 ?        SN   09:03   0:00 [ksmd]
root        17  0.0  0.0      0     0 ?        SN   09:03   0:00 [khugepaged]
root        18  0.0  0.0      0     0 ?        S<   09:03   0:00 [crypto]
root        19  0.0  0.0      0     0 ?        S<   09:03   0:00 [kintegrityd]
root        20  0.0  0.0      0     0 ?        S<   09:03   0:00 [bioset]
root        21  0.0  0.0      0     0 ?        S<   09:03   0:00 [kblockd]
root        22  0.0  0.0      0     0 ?        S<   09:03   0:00 [ata_sff]
root        23  0.0  0.0      0     0 ?        S<   09:03   0:00 [md]
root        24  0.0  0.0      0     0 ?        S<   09:03   0:00 [devfreq_wq]
root        25  0.0  0.0      0     0 ?        S    09:03   0:00 [kworker/u2:1]
root        26  0.0  0.0      0     0 ?        S    09:03   0:00 [kworker/0:1]
root        28  0.0  0.0      0     0 ?        S    09:03   0:00 [kswapd0]
root        29  0.0  0.0      0     0 ?        S<   09:03   0:00 [vmstat]
root        30  0.0  0.0      0     0 ?        S    09:03   0:00 [fsnotify_mark]
root        31  0.0  0.0      0     0 ?        S    09:03   0:00 [ecryptfs-kthrea]
:
```

---

## 查看系统中的每个进程 ##

```
ps -A
ps -e
```
-A：显示所有进程

a：显示终端中包括其它用户的所有进程

x：显示无控制终端的进程

## 显示进程的树状图 ##

```
pstree
```
输出
```
hisen@hisen-server:~$ pstree
systemd─┬─accounts-daemon─┬─{gdbus}
        │                 └─{gmain}
        ├─acpid
        ├─agetty
        ├─atd
        ├─cron
        ├─dbus-daemon
        ├─dhclient
        ├─2*[iscsid]
        ├─java───14*[{java}]
        ├─lvmetad
        ├─lxcfs───2*[{lxcfs}]
        ├─mdadm
        ├─polkitd─┬─{gdbus}
        │         └─{gmain}
        ├─redis-server───2*[{redis-server}]
        ├─rsyslogd─┬─{in:imklog}
        │          ├─{in:imuxsock}
        │          └─{rs:main Q:Reg}
        ├─snapd───5*[{snapd}]
        ├─sshd───sshd───sshd───bash───pstree
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-timesyn───{sd-resolve}
        └─systemd-udevd
```