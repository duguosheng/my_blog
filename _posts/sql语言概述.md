---
title: sql语言概述
date: 2020-02-20 15:21:32
tags:
- sql
categories:
- 数据库
description:
- sql语言用于对数据库的操作
---

<!--more-->

### 查询语句
主要由`select`关键字完成，查询语句是SQL 语句中最复杂、功能最丰富的语句。

### DML语句
Data Manipulation Language，数据操作语言
主要由`insert`、`update`和`delete`三个关键字完成。

### DDL语句
Data Definition Language，数据定义语言
主要由`create`、`alter`、`drop`和`truncate`四个关键字完成。

* ALTER用法
1. 如需在表中添加列，请使用下列语法:
```sql
ALTER TABLE table_name ADD column_name datatype
```

2. 要删除表中的列，请使用下列语法：
```sql
ALTER TABLE table_name DROP COLUMN column_name
```
注释：某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。

3. 要改变表中列的数据类型，请使用下列语法：
```sql
ALTER TABLE table_name ALTER COLUMN column_name datatype
```
或
```sql
ALTER TABLE table_name MODIFY column_name datatype
```

### DCL语句
Data Control Language，数据控制语言
主要`grant`和`revoke` 两个关键字完成。


### 事务控制语句
主要由`commit`、`rollback`和`savepoint`三个关键字完成，`savepoint`了解就行。
