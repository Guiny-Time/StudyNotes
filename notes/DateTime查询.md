---
attachments: ['5TADF2SVFS{{BH_K$`BP(S4.png']
tags: [CS130]
title: TimeStamp/Date/DateTime查询
created: '2021-01-03T16:00:16.982Z'
modified: '2021-01-04T16:48:02.751Z'
---

# TimeStamp/Date/DateTime查询
时间戳和日期都可以直接 = '2017-01-01'或者大于小于啥啥的，datetime和时间戳还可以跟时间的部分
## date_format(列名,'%') = ;
得到的东西可以直接与数字匹配

date_format(downloadts,'%y') 年 
date_format(downloadts,'%m') 月
date_format(downloadts,'%d') 日
date_format(downloadts,'%h') 时
date_format(downloadts,'%i') 分
date_format(downloadts,'%s') 秒
> **注意:**
 %Y表示四位年份，%y表示二位年份
%H表示24小时制，%h表示12小时制

> **关于星期**
星期天是0

## extract(部分 from 列名) = ;
三者通用，部分可以是year/month/hour等等
#### 部分中没有date和time的功能
> **关于星期**
具体的星期无法使用该方法。extract的星期返回的是该年的第几周

## DayofWeek(列名)
星期天是1，从星期天开始

## weekday(列名)
星期一是0，从星期一开始

## TIME_TO_SEC(TIME(列名))
把一切转换成秒

## TO_CHAR(列名,'格式')
这个东西很神经，它和前面所有的都不一样。但是返回值是数字或者字符串
- DY: 返回星期的缩写，比如Mon,Tue,...
- DAY: 返回星期的全称，比如Monday,Tuesday,...
- D: 返回星期的数字，星期天是1
- DD: 日期的数字，1~31
- DDD: 这一天在一年中是第几天，1~366

## 简单粗暴的查询
#### 需要注意的是，用这里的week()返回的是该年的第几周，而不是星期几
#### 有date()和time()功能，返回的是YYYY-MM-DD和HH:MM:SS
<p>
<img src="@attachment/5TADF2SVFS{{BH_K$`BP(S4.png" width="600"></p>
