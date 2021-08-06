---
type: "posts"
author: "jager"
title: "go程序优雅退出"
date: "2021-08-03"
tags: [
"go",
"服务端",
"优雅退出",
]
---

> 本文介绍go程序借助系统信号主动停止服务的实现，即程序优雅退出

<!--more-->

## 原理
通过Notify方法将捕获的signal发送到channel，总而主动停止相关服务
```go
func Notify(c chan<- os.Signal, sig ...os.Signal)
```
用法： 
```go
sig := make(chan os.Signal)  // 创建信号channel
signal.Notify(sig, os.Kill, syscall.SIGINT)  //捕获信号
<-sig  //信号channel阻塞
```



## 代码
```go
package main

import (
	"log"
	"os"
	"os/signal"
	"sync"
	"syscall"
	"time"
)

func main() {
	wait := &sync.WaitGroup{}
	wait.Add(1)
	go func() {
		sig := make(chan os.Signal)
		signal.Notify(sig, os.Kill, os.Interrupt, syscall.SIGINT)
		t := time.Tick(time.Second)
		for {
			select {
			case <-sig:
				log.Printf("Goroutine has exit !")
				wait.Done()
				return

			case <-t:
				log.Printf("Goroutine are running ...")
			}
		}
	}()
	wait.Wait()
}

```
输出
```
2020/03/10 21:19:16 Goroutine are running ...
2020/03/10 21:19:17 Goroutine are running ...
2020/03/10 21:19:18 Goroutine are running ...
2020/03/10 21:19:19 Goroutine are running ...
2020/03/10 21:19:20 Goroutine are running ...
2020/03/10 21:19:21 Goroutine are running ...
2020/03/10 21:19:22 Goroutine are running ...
2020/03/10 21:19:23 Goroutine are running ...
2020/03/10 21:19:24 Goroutine are running ...
^C2020/03/10 21:19:24 Goroutine has exit !
```
