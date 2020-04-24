---
title: distinct和limit关键字
date: 2020-04-15 10:33:18
tags:
- distinct
- limit
categories:
- 数据库
description:
---

### distinct关键字
* 此关键字指定MySql只返回不同的值
<!--more-->

```sql
SELECT vend_id FROM products;
```
![](d1.png)

```sql
SELECT DISTINCT vend_id FROM products;
```
![file](d2.png)

* 不能部分使用distinct
当使用`distinct`时，此关键字应用于所有的列，而不仅是前置它的列
即`SELECT DISTINCT col1, col2 FROM tablename;`命令，除非检索出来的两行`col1, col2`分别都相等，否则全部出现在结果集中，而不能仅仅是`col1`相等。

* 配合使用聚集函数
```sql
SELECT AVG(DISTINCT prod_price)
    AS avg_price
    FROM products;
```

这句语句只求不同值的平均值，如果不使用`DISTINCT`就是所有值的平均值，不使用`DISTINCT`等同于使用`ALL`，`ALL`是默认的

### limit关键字
```sql
-- 指示mysql返回不多于5行
LIMIT 5
-- 指示mysql返回从行3开始返回的5行，以下两行作用等同
LIMIT 3, 5
LIMIT 5 OFFSET 3
```

> 注意mysql检索出的第一行是行0而不是行1，因此`LIMIT 1, 1`将检索出第二行，即行1，而不是第一行 

* 示例
```sql
SELECT vend_id FROM products LIMIT 5 OFFSET 3;
```
![file](d3.png)
