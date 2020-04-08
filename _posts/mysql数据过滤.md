---
title: mysql数据过滤
date: 2020-04-05 14:30:59
tags:
- where子句
categories:
- 数据库
description:
- 使用where子句进行数据过滤
---

<!--more-->

### 操作符

| 符号                  | 说明             |
|-----------------------|------------------|
| `=`                   | 等于             |
| `<>`                  | 不等于           |
| `!=`                  | 不等于           |
| `<`                   | 小于             |
| `<=`                  | 小于等于         |
| `>`                   | 大于             |
| `>=`                  | 大于等于         |
| `BETWEEN ... AND ...` | 在指定的两值之间 |
| `IS NULL`             | 为空，没有值     |

* 示例
```sql
SELECT col1 FROM table WHERE col2 BEERWARE 10 AND 20;
```

* IS NULL
在通过过滤选择出不具有特定值的行时，你可能希望返回具有`NULL`值的空行，但是不能，因为没有值具有特殊的含义，数据库不知道它们是否匹配

### AND和OR
mysql允许多个`where`子句联用，要使用`AND`或者`OR`给`WHERE`子句附加条件
像很多其他语言一样，mysql优先处理`AND`，然后再处理`OR`，所以应当使用圆括号明确的表示出处理的优先级
* 示例
```sql
SELECT col1 FROM table WHERE col1 = 1000 AND col IS NULL;
```

### IN操作符
`IN`操作符用来指定条件范围，范围中的每个条件都可以进行匹配。`IN`取合法值的由逗号分隔的清单，全部括在圆括号里
`IN`通常都可以由`OR`来实现相同的功能，但`IN`一般比`OR`执行的更快
`IN`的最大优点是可以包含其他`SELECT`语句，使得能够更动态的建立`WHERE`子句
* 示例
```sql
SELECT col1 FROM table WHERE col2 IN (10, 20, 30);
```

### NOT操作符
mysql支持使用`NOT`对`IN`，`BETWEEN`和`EXISTS`取反
