---
title: QT中QString和char*互转
date: 2019-12-02 12:41:47
tags:
- QString
categories:
- QT
description:
- 记录一下QT中QT中QString和char*互转
---

<!--more-->

####  QString转char*

```C++
QString qstr = "abcd";
char *str = qstr.toUtf8().data(); //先转为QByteArray,再转为char*
```



#### char*转QString

使用构造方法

```C++
char *ch="hello!";
QString str(ch);
```

