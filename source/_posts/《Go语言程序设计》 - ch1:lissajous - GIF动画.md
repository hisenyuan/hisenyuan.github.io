---
title: 《Go语言程序设计》 - ch1/lissajous - GIF动画
keywords: [go语言,goland,go gif 动画]
date: 2018/9/8 22:08
tags: [go]
categories: go
---
## 一、程序说明
1. 不知道为什么需要http相关内容
2. 直接go run输出的是一丢乱码(图片)
3. 需要go build 然后运行程序才能得到一张图片

具体的内容可以看下面的程序代码(虽然是抄书)
## 二、程序代码
<!--more-->
```go
package main

import (
	"image"
	"image/color"
	"image/gif"
	"io"
	"log"
	"math"
	"math/rand"
	"net/http"
	"os"
	"time"
)

var palette = []color.Color{color.White, color.Black}

const (
	whiteIndex = 0 // 画板中的第一种颜色
	blackIndex = 1 // 画板中的下一种颜色
)

func main() {
	rand.Seed(time.Now().UTC().UnixNano())
	if len(os.Args) > 1 && os.Args[1] == "web" {
		handler := func(w http.ResponseWriter, r *http.Request) {
			lissajous(w)
		}
		http.HandleFunc("/", handler)
		log.Fatal(http.ListenAndServe("localhost:8000", nil))
		return
	}
	lissajous(os.Stdout)
}
func lissajous(out io.Writer) {
	const (
		cycles  = 5     // 完整的x振荡器变化的个数
		res     = 0.001 // 角度分辨率
		size    = 100   // 图像画布包含
		nframes = 64    // 动画中的帧数
		delay   = 8     // 以10ms为单位的帧间延迟
	)

	freq := rand.Float64() * 3.0 // y振荡器的相对频率
	anim := gif.GIF{LoopCount: nframes}
	phase := 0.0 // phase difference
	for i := 1; i < nframes; i++ {
		rect := image.Rect(0, 0, 2*size+1, 2*size+1)
		img := image.NewPaletted(rect, palette)
		for t := 0.0; t < cycles*2*math.Pi; t += res {
			x := math.Sin(t)
			y := math.Sin(t*freq + phase)
			img.SetColorIndex(size+int(x*size+0.5),size+int(y*size+0.5),blackIndex)
		}
		phase +=0.1
		anim.Delay = append(anim.Delay,delay)
		anim.Image = append(anim.Image,img)
	}
	gif.EncodeAll(out,&anim) // 注意：忽略编码错误（直接运行输出的是一堆乱码）
	// go build lissajous.go
	// ./lissajous >out.gif
	// 使用浏览器打开图片就会看到一个动画
}
```
