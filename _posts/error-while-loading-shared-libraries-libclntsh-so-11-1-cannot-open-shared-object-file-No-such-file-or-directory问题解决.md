---
title: >-
  error while loading shared libraries: libclntsh.so.11.1: cannot open shared
  object file: No such file or directory问题解决
date: 2020-02-20 15:54:57
tags:
- oracle
categories:
- problems
description:
- 链接到oracle动态连接库时找不到
---

<!--more-->

[参考文章, 点击跳转](https://blog.csdn.net/red10057/article/details/8202255)

* 首先检查文件是否存在
```bash
# 进入oracle家目录
cd $ORACLE_HOME/lib
# 检查文件是否存在
ls -l |grep libclntsh.so
```

* 如果存在
```bash
# 修改/etc/ld.so.conf添加$ORACLE_HOME/lib路径
sudo vi /etc/ld.so.conf
```

```
include ld.so.conf.d/*.conf
# 在第一行下填写lib路径，使用绝对路径
/u01/oracle/product/11.2.0/db_1/lib/
```

冒号wq保存退出

* 重新ldconfig
```bash
sudo ldconfig
```
