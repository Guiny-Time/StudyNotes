---
tags: [CS130]
title: 视图VIEW
created: '2021-01-04T16:23:12.686Z'
modified: '2021-01-04T17:11:05.634Z'
---

# 视图VIEW
这个东西我也不知道是什么。基于视图可以进行查询、删除、**受限更新**和定义新视图
## 建立视图(基于单表)
#### CREATE VIEW 视图名（列1，列2，...） AS 子查询语句 [WITH CHECK OPTION];
- WITH CHECK OPTION不是必须的，但是如果题目要求受限更新则必须加上
- （列1，列2，...）也不是必须的。这里的列实际上基于子查询语句里查询出的列
- 子查询语句不可以包含ORDER BY和DISTINCT

> **样例:**
CREATE VIEW CS_Student AS 
**SELECT Sno，Sname，Sage FROM Student WHERE Sdept = 'CS'**
WITH CHECK OPTION;
加粗的Select块就是子查询语句，该视图CS_Student由Sno、Sname、Sage三列组成
该视图可能会受到更新修改和插入，需要保证插入的数据的Sdept为CS，所以要加上受限更新的检查选项WITH CHECK OPTION。若Sdept不为CS，则拒绝接受该插入

## 建立视图(基于多表)
#### 区别就是在子查询语句中使用多表联合查询的格式(主键关联)

> **样例**
CREATE VIEW CS_S1(Sno,Sname,Grade) AS 
**SELECT Student.Sno，Sname，Grade FROM Student, SC WHERE Student.Sno = SC.Sno AND SC.Cno = '1' AND Sdept = 'CS'**
WITH CHECK OPTION;

## 带表达式的视图
- 意思是创建视图时，（列1，列2，...）这一堆和底下检索的（列1，列2，...）不一样。对应不上的列被称作虚拟列
- 尽量不要用*来搞

## 分组视图
- 指使用了GROUP BY语句的视图。GROUP BY和ORDER BY类似，**请不要在前面加and一类的东西**
- 对一个列分组之后可以对其使用一些数学操作**function()**，比如求平均值**AVG()**、计数**COUNT()**等等
>**样例**
SELECT 列1, function(可以瞎取)
FROM 表名
GROUP BY 列某;
指对列某进行function操作，结果打印的是列1和function（瞎取的名字），fun这个底下的是对列某操作的数据
我写的有点乱七八糟的，就是那个意思（

## 关于子查询语句
- 只要你想，就没有做不到的事...比如在where后面加个**正则**或者**模糊查询**玩玩
- 多表一定要联合，我估计八成会考多表视图，不然太没意思了
