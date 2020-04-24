---
title: mysql分组数据
date: 2020-04-23 09:49:02
tags:
- mysql
categories:
- 数据库
description:
- 分组是在`SELECT`语句的`ORDER BY`子句中建立的
---

<!--more-->
## 创建分组
分组是在`SELECT`语句的`ORDER BY`子句中建立的

```sql
SELECT vend_id, COUNT(*) AS num_prods
	FROM products
	GROUP BY vend_id;
```

![file](m1.png)

通过`GROUP BY`子句，导致对每个vend_id而不是整个表计算`COUNT(*)`一次

除了聚集计算语句外，`SELECT`语句中的每个列都必须在`GROUP BY`子句中给出

`GROUP BY`必须出现在`WHERE`之后，`ORDER BY`之前

* WITH ROLLUP关键字

在group分组字段的基础上再进行统计数据

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id WITH ROLLUP;
```

![file](m2.png)

## 过滤分组
过滤分组使用`HAVING`子句，他非常类似于`WHERE`，唯一的差别是`WHERE`过滤行，而`HAVING`过滤分组(`WHERE`在分组前进行过滤，而`HAVING`在数据分组后进行过滤)

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id
HAVING COUNT(*) > 2;
```

![file](m3.png)
