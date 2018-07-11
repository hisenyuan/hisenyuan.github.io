---
title: Git Bash tree（windows）让windows支持tree
keywords: [Git Bash tree]
date: 2017/7/13 1:24
tags: [linux]
categories: linux
---
在安装了git客户端之后，发现Git Bash挺好用

之前在linux shell用过tree命令感觉不错

发现Git Bash也可以实现，于是就记录一下

下载地址：<a href="http://gnuwin32.sourceforge.net/packages/tree.htm" target="_blank">点击前往</a>

下载文件： Binaries	 	Zip

解压文件：bin目录下找到tree.exe

把这个放到git安装目录下后的路径：C:\Program Files\Git\usr\bin\tree.exe

测试：虽然有点丑。。。
<!--more-->
```
hisen@HiSEN MINGW64 /c/code/SpringBootCLI/myapp
$ tree
.
|-- mvnw
|-- mvnw.cmd
|-- pom.xml
`-- src
    |-- main
    |   |-- java
    |   |   `-- com
    |   |       `-- example
    |   |           `-- myapp
    |   |               |-- DemoApplication.java
    |   |               `-- ServletInitializer.java
    |   `-- resources
    |       |-- application.properties
    |       |-- static
    |       `-- templates
    `-- test
        `-- java
            `-- com
                `-- example
                    `-- myapp
                        `-- DemoApplicationTests.java

14 directories, 7 files
```