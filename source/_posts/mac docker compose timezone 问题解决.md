---
title: mac docker compose timezone 问题解决
keywords: [mac docker /usr/share/zoneinfo/Asia/Shanghai,mac docker /etc/localtime,docker]
date: 2018/12/21 23:26
tags: [docker]
categories: docker
---
# 一、背景介绍
想用docker搭建一个lnmp环境

使用这个脚本：
```
https://github.com/buxiaomo/docker-compose/tree/master/lnmp
```

执行命令之后报错
```
docker-compose -f lnmp.yml up -d
WARNING: Some services (mysql, nginx, php, redis) use the 'deploy' key, which will be ignored. Compose does not support 'deploy' configuration - use `docker stack deploy` to deploy to a swarm.
WARNING: Some services (mysql, nginx, php) use the 'configs' key, which will be ignored. Compose does not support 'configs' configuration - use `docker stack deploy` to deploy to a swarm.
lnmp_mysql_1 is up-to-date
Starting lnmp_redis_1 ...
lnmp_nginx_1 is up-to-date
Starting lnmp_redis_1 ... error

ERROR: for lnmp_redis_1  Cannot start service redis: b'Mounts denied: \r\nThe path /usr/share/zoneinfo/Asia/Shanghai\r\nis not shared from OS X and is not known to Docker.\r\nYou can configure shared paths from Docker -> Preferences... -> File Sharing.\r\nSee https://docs.docker.com/docker-for-mac/osxfs/#namespaces for more info.\r\n.'

                    memory: 100M
ERROR: for redis  Cannot start service redis: b'Mounts denied: \r\nThe path /usr/share/zoneinfo/Asia/Shanghai\r\nis not shared from OS X and is not known to Docker.\r\nYou can configure shared paths from Docker -> Preferences... -> File Sharing.\r\nSee https://docs.docker.com/docker-for-mac/osxfs/#namespaces for more info.\r\n.'
ERROR: Encountered errors while bringing up the project.
```

# 二、报错原因
1. mac机器的时间路径与linux不一样
2. lnmp.yml redis用到了这个时间

# 三、解决办法
1. 查看当前机器时间文件真实位置(/etc/localtime 这个路径是不让共享给docker的)
```
ls -la /etc/localtime
lrwxr-xr-x  1 root  wheel  39 10 19 11:13 /etc/localtime -> /var/db/timezone/zoneinfo/Asia/Shanghai
```
2. 设置位置 ：Docker -> Preferences... -> File Sharing
3. 新增共享目录：/var/db/timezone
4. apply & restart
5. 修改lnmp.yml配置文件 全局替换：/usr/share/zoneinfo/Asia/Shanghai 为：/var/db/timezone/zoneinfo/Asia/Shanghai

# 四、成功信息
```
docker-compose -f lnmp.yml up -d
WARNING: Some services (mysql, nginx, php, redis) use the 'deploy' key, which will be ignored. Compose does not support 'deploy' configuration - use `docker stack deploy` to deploy to a swarm.
WARNING: Some services (mysql, nginx, php) use the 'configs' key, which will be ignored. Compose does not support 'configs' configuration - use `docker stack deploy` to deploy to a swarm.
Removing lnmp_redis_1
Starting lnmp_nginx_1                ... done
Starting lnmp_mysql_1                ... done
Recreating f5b9db566588_lnmp_redis_1 ... done
Starting lnmp_php_1                  ... done
```
