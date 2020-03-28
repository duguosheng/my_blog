---
title: Oracle的序列生成器
date: 2020-02-26 09:13:37
tags:
- 序列生成器
- oracle
categories:
- 数据库
description:
- 用于生成递增或递减的序列
---

<!--more-->
序列(SEQUENCE)是序列号生成器，可以为表中的行自动生成序列号，产生一组等间隔的数值(类型为数字)。不占用磁盘空间，占用内存。其可以在插入语句中引用，也可以通过查询检查当前值，或使序列增至下一个值。
* 创建序列生成器

```sql
CREATE SEQUENCE SEQ _SURFDATA INCREMENT BY 1 START WITH 100 MAXVALUE 9999999999 NOCYCLE NOCACHE;
```

语法详解：
```sql
--创建序列命令
CREATE SEQUENCE 序列名
--定义序列的步长（增长量），如果省略，则默认为1
--如果出现负值，则代表序列的值是按照此步长递减的。
[INCREMENT BY n]
--定义序列的初始值(即产生的第一个值)，默认为1
[START WITH n]
--定义序列生成器能产生的最大值
--选项NOMAXVALUE是默认选项，代表没有最大值定义
--这时对于递增序列，系统能够产生的最大值是10的27次方;对于递减序列，最大值是-1。
--MINVALUE 定义序列生成器能产生的最小值
--选项NOMAXVALUE是默认选项，代表没有最小值定义
--这时对于递减序列，系统能够产生的最小值是?10的26次方;对于递增序列，最小值是1。
[{MAXVALUE/MINVALUE n|NOMAXVALUE}] --3、
--CYCLE 和 NOCYCLE 表示当序列生成器的值达到限制值后是否循环
--CYCLE代表循环，NOCYCLE代表不循环。
[{CYCLE|NOCYCLE}]
--CACHE(缓冲)定义存放序列的内存块的大小，默认为20。NOCACHE表示不对序列进行内存缓冲。
--对序列进行内存缓冲，可以改善序列的性能
[{CACHE n|NOCACHE}]
```

* 删除序列
```sql
DROP SEQUENCE 序列名;
```

* 获取序列的当前值和下一个值
```sql
--获取序列的当前值
SELECT 序列名.CURRVAL FROM dual;
--获取序列的下一个值
SELECT abc.NEXTVAL FROM dual;
```

