---
title: oracle中的表及其相关操作
date: 2020-02-20 15:25:01
tags:
- oracle
categories:
- 数据库
description:
- 表示数据库基本的结构
---

<!--more-->
## 表的概念
### 基本理解
表是数据库的最基本的逻辑结构，一切数据都存放在表中，其它数据库对象（索引、视图、同义词等）都是为了用户很好地操作表中的数据，一个ORACLE数据库就是由若干个数据表组成，每个表由列和行组成，如下表所示。

|学号|姓名|性别|籍贯|
|-  |   -|-   |-   |
|001|张三 | 男 |北京|
|002|李四 | 男 |上海|

### 表的组成
> 表是由列或字段组成，表的内容，也就是表的各个属性，组成了表的各个列。

* 表结构
1. 列名：列的名称。
2. 长度：该列所能容纳的最大数据位数。
3. 数据类型：该列存储的数据类型，常用数据类型如所示。
4. 非空列：该列值是不能为空的。
5. 主键：该列能唯一表示一行内容，则称该列为关键字。识别码，唯一的ID。

### 常用数据类型
|数据类型|说明|最大长度|表示方法|
|    -   | - |   -   |   -    |
|char(长度)|定长字符串|255|单引号括起来，如'张三'|
|varchar2(长度)|变长字符串|2000|同上|
|number(长度,精度)|数字|38|可以括起来，也可以不括，如12.3或'12.3'|
|date|日期时间，实际是一个整数||用`to_date`和`to_char`把字符串和日期进行转换|

`to_date`使用：如`to_date('2020-01-01 20:00:00','yyyy-mm-dd hh24:mi:ss')`将字符串转换为`date`类型

`to_char`使用：假设`birthday`是表中的一个`date`数据类型，使用`to_char(birthday,'yyyy-mm-dd hh24:mi:ss')`将`date`类型转换为`yyyy-mm-dd hh24:mi:ss`格式的字符串


**行/记录**：表中所有列组合在一起形成的一条信息，称之为一行或一条记录。

---

## 表的操作
> sql命令不区分大小写
> sql中使用两个短横线作为注释符号，如

```sql
--这是一段注释
```

### 创建表
* 命令
```sql
CREATE TABLE tablename
	(
	--每个参数间以逗号结束
	--变量名 数据类型
	column1 datatype,
	--null表示可以空，为默认值，可不填
	column2 datatype null,
	--not null表示该项必须非空
	column3 datatype not null,
	……,
	--最后一行没有逗号
	--指定id为主键
	primary key(id)
	);
```

* 建表举例
```sql
create table T_GIRL
(
    id char(6),
    name varchar2(30) not null,
    gender char(2),
    hometown varchar2(100)
);
```

### 表记录的插入
```sql
--第一种写法,前后的值要一一对应
insert into tablename(col1,col2,col3,……,coln) values(value1,value2,value3,……,valuen);
--还有一种写法。
insert into tablename values(value1,value2,value3,……,valuen);--value的顺序为建表时指定的顺序
```
后面的写法一定不能出现在程序中，因为只要表结构发生改变，或字段的位置改变，SQL就会出错。

### 查询表中数据
* 查询
```sql
--常用语法
select col1,col2,col3 from tablename where ...; --where后接条件语句
--查询法和条件的所有内容
select * from tablename where ...;
--查询系统时间
select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual;
--查询表的结构
desc tablename;
```

**条件判断**:`=`相等, `!=`不相等, `and`与, `or`或

* 排序
```sql
--在上面的语句后接
order by A; --查询后根据A元素升序排序
order by A,B; --查询后先根据A升序,再根据B元素升序排序
order by A asc; --asc表示升序,为默认值,可省略
order by A desc; --desc是降序
order by A,B desc; --先根据A升序,再根据B降序排列
order by A desc, B desc; --现根据A降序,再根据B降序排
```

### 修改表中的数据
```sql
update tablename set col1=value1,col2=value2,……,coln=valuen where 条件表达式;
```

如果没有条件表达式，就更新表中全部的数据。

### 删除表中的数据
```sql
delete from tablename where 条件表达式;
```

如果没有条件表达式，就删除表中全部的数据。

## sqlplus技巧
在sqlplus登录后,执行sql语句,可以用`/`重复上一条命令,`l`显示出来上一条语句,`c/old/new`来将上一条命令中的`old`改为`new`
