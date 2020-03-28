---
title: Oracle的虚表dual及伪列
date: 2020-02-26 08:53:22
tags:
- oracle
- dual
categories:
- 数据库
description:
- dual是一个虚拟表，用来构成select的语法规则
---

<!--more-->
## 虚表dual

dual是一个虚拟表，用来构成select的语法规则，oracle保证dual里面永远只有一条记录。我们可以用它来做很多事情，如下：
* 查看当前用户，可以在 SQL Plus中执行下面语句

```sql
select user from dual;
```

* 用来调用系统函数

```sql
--获得当前系统时间
select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual;
--获得主机名
select SYS_CONTEXT('USERENV','TERMINAL') from dual;
--获得当前 locale
select SYS_CONTEXT('USERENV','language') from dual;
--获得一个随机数
select dbms_random.random from dual;
```
3、得到序列的下一个值或当前值，用下面语句

```sql
--获得序列your_sequence的下一个值
select your_sequence.nextval from dual;
--获得序列your_sequence的当前值
select your_sequence.currval from dual;
```

4、可以用做计算器

```sql
select 7*9 from dual;
```

> C语言取的是操作系统的时间
> Oracle运行数据库的服务器的操作系统的时间


## 伪列
Oracle 中伪列就像一个表列（表中的列），但是它并没有存储在表中，伪列可以从表中查询，但不能插入、更新和删除它们的值
常用的伪列有ROWID和ROWNUM。

* ROWNUM 是查询返回的结果集中行的序号，可以使用它来限制查询返回的行数。

* rowid格式
ROWID是数据的详细地址，通过rowid，oracle可以快速的定位某行具体的数据在磁盘中存放的物理位置。
```sql
--查询rowid
select rowid from 表名;
```

查到的格式如`AAASP3AAEAAAACvABj`
第一部分6位表示：该行数据所在的数据对象的编号；
第二部分3位表示：该行数据所在的相对数据文件的id;
第三部分6位表示：该数据行所在的数据块的编号；
第四部分3位表示：该行数据的行的编号；

> 使用rowid可以快速查询定位文件，效率很高
```sql
select * from 表名 where rowid='AAASP3AAEAAAACvABj';
```

