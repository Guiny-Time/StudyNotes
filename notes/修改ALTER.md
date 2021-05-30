---
tags: [CS130]
title: 修改ALTER
created: '2021-01-04T12:35:05.538Z'
modified: '2021-01-04T13:51:43.974Z'
---

# 修改ALTER
## 修改数据库

#### RENAME DATABASE 数据库A TO 数据库B

把“数据库A”这个数据库的名字改成了“数据库B”
## 删除数据库
#### DROP DATABASE 数据库名;
## 修改表名

#### ALTER TABLE 表A RENAME TO 表B;

把“表A”这个表的名字改成了“表B”

## 修改列名

#### ALTER TABLE 表名 RENAME 列A TO 列B
把“列A”这个列的名字改成了“列B”

## 修改列的数据类型

#### ALTER TABLE 表名 ALTER COLUMN 列名 数据类型
把对应列的数据类型改成上面写的数据类型

## 删除列

#### ALTER TABLE 表名 DROP COLUMN 列名
删除对应列

## 增加列

#### ALTER TABLE 表名 ADD 列名 数据类型
增加了一个数据类型为数据类型的列
