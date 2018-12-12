---
title: go程序设计语言练习 - 同统计重复 - gop1.io/ch1/dup2/dup2.go
keywords: [go,dup2,统计,去重]
date: 2018/10/27 13:32
tags: [go]
categories: go
---
一直在写java，看语法逻辑什么的没有问题

但是刚刚看书上写出来的这个程序，居然不知道怎么在goland里面运行...

于是曲线救国，了解到打印输入的参数。

这断代码就是为了找出输入数据中的重复行

1. 直接启动，不带参数，启动之后输入参数
```
参数0: /private/var/folders/lk/p8gbq87n6rvfndk7wlk23y8r0000gn/T/___go_build_dup2_go
1 # 一行输入一个
1 # 一行输入一个
^D #这是command + D
2	1 # 输出的结果：个数 输入值
```
2. 在命令行启动，带
```
$ go run dup2.go 'hisen.txt'
参数0: /var/folders/lk/p8gbq87n6rvfndk7wlk23y8r0000gn/T/go-build525680776/b001/exe/dup2
参数1: hisen.txt #代码同级目录的文件名
2       hisen
2       hisenyuan
2       123
```

代码如下：
<!--more-->
```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

/**
本程序只能在终端运行：go run dup2.go 'hisen.txt'
hisen.txt 为本文件同级目录的文件
 */
func main() {
	// 遍历输出的参数
	for idx, args := range os.Args {
		fmt.Println("参数"+strconv.Itoa(idx)+":", args)
	}
	// map[string,int]
	counts := make(map[string]int)
	// 拿到文件名，从第二个参数开始
	files := os.Args[1:]
	if len(files) == 0 {
		countLines(os.Stdin, counts)
	} else {
		// 遍历文件
		for _, arg := range files {
			// 读取文件内容
			f, err := os.Open(arg)
			if err != nil {
				fmt.Fprintf(os.Stderr, "dup2: %v\n", err)
				continue
			}
			countLines(f, counts)
			f.Close()
		}
	}

	// 遍历map
	for line, n := range counts {
		if n > 1 {
			fmt.Printf("%d\t%s\n", n, line)
		}
	}
}

// 把数据装入map
func countLines(file *os.File, counts map[string]int) {
	// 从文件中加载数据
	input := bufio.NewScanner(file)
	// 遍历文件内容
	for input.Scan() {
		counts[input.Text()]++
	}
}
```
