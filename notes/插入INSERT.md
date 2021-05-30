---
tags: [CS130]
title: 插入INSERT
created: '2021-01-04T05:59:32.069Z'
modified: '2021-01-04T16:18:13.030Z'
---

# 插入INSERT

## 单表插入
#### INSERT INTO 表名 (列1，列2，列3，...) VALUE (值1,值2,值3,...);
- 注意！如果是字符串的话需要用''罩住信息

## 联合插入
#### INSERT INTO 表名 （列1，列2，列3,...) SELECT 列1，列2，列3，... FROM 现成表 GROUP BY 列1
