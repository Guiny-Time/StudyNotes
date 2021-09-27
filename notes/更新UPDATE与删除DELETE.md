---
tags: [大二/CS130数据库]
title: 更新UPDATE与删除DELETE
created: '2021-01-04T07:42:41.097Z'
modified: '2021-09-27T02:16:44.512Z'
---

# 更新UPDATE与删除DELETE

## 单表更新
#### UPDATE 表名 SET 列名 = 新值 WHERE 条件
- 老生常谈，如果是字符串要用''包起来
## 单表删除
#### DELETE FROM 表名 WHERE 条件
- 要删就是删掉一整行
#### DROP TABLE 表名
- 要删就是删掉一整个表
## 多表更新
#### UPDATE 表1 LEFT JOIN 表2 ON 主键相等 SET 列名 = 新值 WHERE 条件
- 使用left join是因为中间表依赖两个大表而存在，当我们做搜索的时候是基于大表的，所以是大表left join中间表
- 使用inner join会导致查出一些不存在于大表中但是存在于中间表的行
## 多表删除
#### DELETE 表1，表2，... FROM 表1，表2，... WHERE 主键相等 AND 条件
- 多表联合删除若要用这种格式来写的话，不能用将表1 as a这种简写写法，但是update可以
- 另一种多表删除的写法见下面的**特殊函数IN**，这也是老师用的写法
## 级联删除
#### 先删除中间表中的有关记录，再删除大表格
- DELETE 中间表 FROM 中间表，大表，... WHERE 级联主键相等；
- DROP TABLE 大表；

## 特殊函数
#### IN
这个可以用在insert/update和delete里。其目的是提取要删除的东西的主键
**DELETE的例子**
> DELETE FROM LabExam2_Visited 
  WHERE BBID IN 
  ( SELECT BBID  FROM LabExam2_People WHERE Customer In ('Paul Hunt','Deborah Kelley') );

> DELETE FROM LabExam2_People 
WHERE Customer IN 
('Paul Hunt','Deborah Kelley');

**UPDATE的例子**
> UPDATE SC SET Grade = 0 
WHERE Sno IN 
(SELECT Sno FROM Student WHERE Sdept = 'CS');


