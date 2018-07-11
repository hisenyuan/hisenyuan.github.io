---
title: 如何检测Linux VPS系统架构是Xen、KVM还是OpenVZ
date: 2017-02-08 21:12:06
tags: [linux]
---
结果
---
```
[root]# virt-what
xen
xen-hvm
```

Centos
---
```
wget http://people.redhat.com/~rjones/virt-what/files/virt-what-1.12.tar.gz
tar zxvf virt-what-1.12.tar.gz
cd virt-what-1.12/
./configure
make && make install
virt-what
```

Ubuntu/debian
---
```
apt-get install virt-what
virt-what
```