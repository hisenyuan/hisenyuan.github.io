---
title: linux查看日志文件常用命令
keywords: [linux查看日志,linux log常用命令]
date: 2017/4/7 16:16
tags: [linux]
categories: linux
---
linux查看日志文件内容命令tail、cat、tac、head、echo

tail -f test.log

你会看到屏幕不断有内容被打印出来. 这时候中断第一个进程Ctrl-C,

cat -n hisen.log | grep '907'

在文件当中查找指定的内容，这里是查询：907

---------------------------
linux 如何显示一个文件的某几行(中间几行)

从第3000行开始，显示1000行。即显示3000~3999行
```
cat filename | tail -n +3000 | head -n 1000
```
显示1000行到3000行
```
cat filename| head -n 3000 | tail -n +1000
```
*注意两种方法的顺序
分解：

1. tail -n 1000：显示最后1000行
2. tail -n +1000：从1000行开始显示，显示1000行以后的
3. head -n 1000：显示前面1000行

用sed命令
sed -n '5,10p' filename 这样你就可以只查看文件的第5行到第10行。

例：cat mylog.log | tail -n 1000 #输出mylog.log 文件最后一千行

---------------------------
cat主要有三大功能：

1.一次显示整个文件。$ cat filename

2.从键盘创建一个文件。$ cat > filename 

只能创建新文件,不能编辑已有文件.

3.将几个文件合并为一个文件： $cat file1 file2 > file

参数：

-n 或 --number 由 1 开始对所有输出的行数编号

-b 或 --number-nonblank 和 -n 相似，只不过对于空白行不编号

-s 或 --squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行

-v 或 --show-nonprinting

例：

把 textfile1 的档案内容加上行号后输入 textfile2 这个档案里
```
cat -n textfile1 > textfile2
```
把 textfile1 和 textfile2 的档案内容加上行号（空白行不加）之后将内容附加到 textfile3 里。
```
cat -b textfile1 textfile2 >> textfile3
```
把test.txt文件扔进垃圾箱，赋空值test.txt
```
cat /dev/null > /etc/test.txt 
```

注意：>意思是创建，>>是追加。千万不要弄混了。

---

tac (反向列示)

tac 是将 cat 反写过来，所以他的功能就跟 cat 相反， cat 是由第一行到最后一行连续显示在萤幕上，

而 tac 则是由最后一行到第一行反向在萤幕上显示出来！

------------------------------------------
在Linux中echo命令用来在标准输出上显示一段字符，比如：
echo "the echo command test!"

这个就会输出“the echo command test!”这一行文字！

echo "the echo command test!">a.sh

这个就会在a.sh文件中输出“the echo command test!”这一行文字！ 

该命令的一般格式为： echo [ -n ] 字符串其中选项n表示输出文字后不换行；字符串能加引号，也能不加引号。

用echo命令输出加引号的字符串时，将字符串原样输出；

用echo命令输出不加引号的字符串时，将字符串中的各个单词作为字符串输出，各字符串之间用一个空格分割。

一些实例
---
<!--more-->
```
#在整个文件搜索含有907的内容
hisen@ubuntu:~/dl$ cat -n hisen.log | grep '907'
    25	[2017-04-07 03:49:04,907] Artifact ssm_study:war exploded: Art
    26	[2017-04-07 03:49:04,907] Artifact ssm_study:war exploded: Dep

#从第3行开始,显示10行 即：3~12 并且显示行号
hisen@ubuntu:~/dl$ cat -n hisen.log | tail -n +3 | head -n 10
     3	07-Apr-2017 15:48:50.072 信息 [main] org.apache.coyote.Abstrac
     4	07-Apr-2017 15:48:50.137 信息 [main] Using a shared selector f
     5	07-Apr-2017 15:48:50.172 测试 [main] Initializing ProtocolHand
     6	07-Apr-2017 15:48:50.186 信息 [main] Using a shared selector f
     7	07-Apr-2017 15:48:50.187 你猜 [main] load Initialization proce
     8	07-Apr-2017 15:48:50.261 信息 [main] StandardService.startInte
     9	07-Apr-2017 15:48:50.261 信息 [main] Servlet Engine: Apache To
    10	07-Apr-2017 15:48:50.293 信息 [main] Starting ProtocolHandler
    11	07-Apr-2017 15:48:50.318 哪里 [main] ProtocolHandler [ajp-nio-
    12	07-Apr-2017 15:48:50.328 信息 [main] start Server startup in 1
#显示10行前三行
hisen@ubuntu:~/dl$ cat -n hisen.log | head -n +10 | tail -n 3
     8	07-Apr-2017 15:48:50.261 信息 [main] StandardService.startInte
     9	07-Apr-2017 15:48:50.261 信息 [main] Servlet Engine: Apache To
    10	07-Apr-2017 15:48:50.293 信息 [main] Starting ProtocolHandler

#显示3-10行
hisen@ubuntu:~/dl$ cat -n hisen.log | head -n 10 | tail -n +3
     3	07-Apr-2017 15:48:50.072 信息 [main] org.apache.coyote.Abstrac
     4	07-Apr-2017 15:48:50.137 信息 [main] Using a shared selector f
     5	07-Apr-2017 15:48:50.172 测试 [main] Initializing ProtocolHand
     6	07-Apr-2017 15:48:50.186 信息 [main] Using a shared selector f
     7	07-Apr-2017 15:48:50.187 你猜 [main] load Initialization proce
     8	07-Apr-2017 15:48:50.261 信息 [main] StandardService.startInte
     9	07-Apr-2017 15:48:50.261 信息 [main] Servlet Engine: Apache To
    10	07-Apr-2017 15:48:50.293 信息 [main] Starting ProtocolHandler

#显示倒数第二行
hisen@ubuntu:~/dl$ cat -n hisen.log | tail -n 2
    25	[2017-04-07 03:49:04,907] Artifact ssm_study:war exploded: Art
    26	[2017-04-07 03:49:04,907] Artifact ssm_study:war exploded: Dep

#从24行开始，显示到最后
hisen@ubuntu:~/dl$ cat -n hisen.log | tail -n +24
    24	07-Apr-2017 15:49:04.585 信息 during time.
    25	[2017-04-07 03:49:04,907] Artifact ssm_study:war exploded: Art
    26	[2017-04-07 03:49:04,907] Artifact ssm_study:war exploded: Dep
```