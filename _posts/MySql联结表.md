---
title: MySql联结表
date: 2020-04-29 09:35:14
tags:
- mysql
- 联结表
categories:
- 数据库
---


## 联结表
分解数据为多个表能更有效地存储，更方便的处理，比如学生的信息和学生选课的信息，存在两个表更为方便，但这些好处是有代价的。
<!--more-->
如果数据存储在多个表中，怎样用单条`SELECT`语句检索出数据？
答案是使用**联结**。

> 简单来说，联结是一种机制，用来在一条`SELECT`语句中关联表，因此称之为联结。使用特殊的语法，可以联结多个表返回一组输出

### 创建联结
联结的创建非常简单，规定要联结的所有表及其如何关联即可
```sql
SELECT vend_name, prod_name, prod_price
FROM vendors AS v, products AS p
WHERE v.vend_id = p.vend_id
ORDER BY vend_name, prod_name;
```

![file](http://www.greatboy.xyz/wp-content/uploads/2020/04/image-1588069934633.png)

查询中指定的三个列中，有两个列在一张表中，另一个列在另一个表中，这两个表用`WHERE`正确联结，应当保证所有的联结都有`WHERE`子句

### 内部联结
目前为止所用的联结称为等值联结，它基于两个表之间的相等测试，。这种联结也称为内部联结。这种联结还可以使用稍微不同的语法，下面的例子返回与前面的例子完全相同
```sql
SELECT vend_name, prod_name, prod_price
FROM vendors AS v INNER JOIN products AS p
ON v.vend_id = p.vend_id
ORDER BY vend_name, prod_name;
```

两个表用`INNER JOIN`指定，条件用`ON`指定而不是`WHERE`

> ANSI SQL规范首选`INNER JOIN`语法

### 联结多个表
```sql
SELECT prod_name, vend_name, prod_price, quantity
FROM orderitems AS o, products AS p, vendors AS v
WHERE v.vend_id = p.vend_id
  AND o.prod_id = p.prod_id
  AND order_num = 20005;
```

## 创建高级联结
### 自联结
假如发现物品ID为DTNTR存在问题，想知道生产该物品的供应商生产的其他物品是否也存在这些问题。此查询要求首先查询到DTNTR的供应商，再查询其生产的其他产品

* 使用子查询
```sql
SELECT prod_id, prod_name
FROM products
WHERE vend_id = (SELECT vend_id
				 FROM products
                 WHERE prod_id = 'DTNTR');
```

* 使用自联结
```sql
SELECT p1.prod_id, p1.prod_name
FROM products AS p1 INNER JOIN products AS p2
ON p2.prod_id = 'DTNTR'
AND p1.vend_id = p2.vend_id;
```

此查询使用的两个表实际上是同一张表，通过使用别名消除了二义性

### 自然联结
无论何时对表联结，应该至少有一个列出现在不止一个表中（被联结的列）。标准的联结返回所有数据，**甚至相同的列出现多次**，自然联结排除多次出现，使每个列只返回一次。
怎样完成这项工作呢？答案是由你自己完成。**你只能选择那些唯一的列**。这一般是通过对表使用通配符(`SELECT *`)，对所有其他表的列使用明确的子集（防止出现重复列）来完成的。
前面所使用的联结都是自然联结，也许永远都用不到非自然联结。

### 外部联结
许多联结将一个表中的行与另一个表中的行相关联，但有时需要包含没有关联行的那些行，例如：对每个客户下了多少订单计数，包括至今未下单的客户

**联结包含了那些在相关表中没有关联行的行，这种联结称为外部联结**

* 内部联结和外部联结对比

```sql
-- 内部联结
SELECT c.cust_id, o.order_num
FROM customers AS c INNER JOIN orders AS o
  ON c.cust_id = o.cust_id;
```

![file](http://www.greatboy.xyz/wp-content/uploads/2020/04/image-1588083832395.png)

```sql
-- 外部联结
SELECT c.cust_id, o.order_num
FROM customers AS c LEFT OUTER JOIN orders AS o
  ON c.cust_id = o.cust_id;
```

![file](http://www.greatboy.xyz/wp-content/uploads/2020/04/image-1588083853817.png)

可见，外部联结可以返回没有关联行的行

* 使用外部联结
外部联结使用`OUTER JOIN`指定联结的类型，同时也必须使用`LEFT`或`RIGHT`指定包括其所有行的表

`A LEFT OUTER JOIN B`，A的所有行在B中检索，如果B无匹配项，就返回NULL
`A RIGHT OUTER JOIN B`，B的所有行在A中检索，如果A无匹配项，就返回NULL
