---
title: mac安装Go环境 & IDE(GoLand) & 基本运行方法(标准输入)
keywords: [mac安装go,goland,go输入流运行]
date: 2018/9/8 11:05
tags: [go]
categories: go
---
## 一、安装GO
1.1 使用Homebrew安装go环境(如果很慢，可以换个源)
```
brew install go
```
1.2 查看安装信息
```
go env
```
主要关注如下输出
```
GOROOT="/usr/local/Cellar/go/1.10.3/libexec" # 安装目录
```
1.3 配置环境变量
```
vi ~/.bash_profile # 没有的话会新建一个文件
```
输入如下内容，第一行是安装的目录，第三行是工作目录(可以改成自己喜欢的路径)
```
GOROOT=/usr/local/Cellar/go/1.10.3/libexec
export GOROOT
export GOPATH=/Users/hisenyuan/golang
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN:$GOROOT/bin
```
1.4 让配置文件生效并且查看环境变量
```
source ~/.bash_profile
go env # 这时你会发现环境变量已经有改变
```

## 二、安装GoLand
我是习惯了用jetbrains的idea

发现它家也有go语言的IDE GoLand

于是就去官网下载，安装，找个注册码，修改一下host防止注册码失效

这里就不再累赘了

## 三、运行需要输入的程序
买了一本《Go语言程序设计》
<!--more-->
有些程序需要从标准输入获取信息，然后进行处理，例如dup2

运行之后不知道该肿么办，百度一番之后发现了方法

命令行：Ctl + D

GoLand：command + D

贴一段程序和运行的结果
```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

// dup2 打印输入中多次出现的文本和行数
// 它从stdin或指定的文件到文件列表读取

func main() {
	counts := make(map[string]int)
	files := os.Args[1:]
	if len(files) == 0 {
		countLines(os.Stdin, counts)
	} else {
		for _, arg := range files {
			f, err := os.Open(arg)
			if err != nil {
				fmt.Fprintf(os.Stderr, "dup2: %v\n", err)
				continue
			}
			countLines(f, counts)
			f.Close()
		}
	}
	for line, n := range counts {
		if n > 1 {
			fmt.Printf("%d\t%s\n", n, line)
		}
	}
}
func countLines(f *os.File, counts map[string]int) {
	input := bufio.NewScanner(f)
	for input.Scan(){
		counts[input.Text()]++
	}
}
// 执行的过程中，得按command d
//123
//123
//123
//111
//222
//222
//112
//111^D
//3	123
//2	222
```
