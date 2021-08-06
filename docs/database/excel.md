---
type: "posts"
author: "jager"
title: "Excel表的操作记录"
date: "2021-08-06"
tags: [
"excel",
]
---

> 本文记录Excel表的一些记录，供必要时查阅

<!--more-->

+ Excel 时间戳转时间字符串
```
  =TEXT((C2+8*3600)/86400+70*365+19,"yyyy-mm-dd hh:mm:ss")
```