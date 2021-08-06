---
type: "posts"
author: "jager"
title: "MongoDB的一些操作记录"
date: "2021-08-06"
tags: ["mongodb"]
---

> 本文记录MongoDB的一些操作记录，如导出数据备份数据，创建用户等。

<!--more-->

+ MongoDB导出数据（Excel）
  ``mongoexport -d jygame -u juyou -p JuYouServer -c players -f _id,data.ip,data.createTime,data.lastLoginTime,data.resource.1,data.resource.2,data.resource.4,data.VideoTotal,data.withdraw.withdrawTotal --type=csv -o ./data.csv``

+ MongoDB创建用户
  ``db.createUser({user:'juyou', pwd:'JuYouServer', roles:[{ role: "readWrite", db: "jygame" }]})``

+ MongoDB备份数据
  ``mongodump -d jygame_ym -o /root/workspace/mongo/backup -u juyou -p JuYouServer``