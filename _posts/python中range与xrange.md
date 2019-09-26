---
title: python中range与xrange
date: 2019-09-24 10:32:57
tags:
- python基础
categories:
- python
description: 在python2中存在`xrange()`，python3中`range()`的实现方式与`xrange()`相同，所以就不再有`xrange()`
---

<!--more-->

## 用法
* `range(start, stop, step)`与`xrange(start, stop, step)`用法相同
  * start：起始数字，可不填，默认0
  * stop：结束数字，必须填
  * step：步长，可不填，默认1

## 区别
* `range()`直接生成一个列表，需要立即开辟内存空间
* `xrange()`生成一个可以迭代的对象，即用即取
* 要生成很大的数字序列的时候，用xrange会比range性能优很多，因为不需要一上来就开辟一块很大的内存空间

```py
#! /usr/bin/python2
# *-* coding: utf-8 *-*
print("测试range")
my_range = range(10)
print(my_range)
print("测试xrange")
my_xrange = xrange(10)
print(my_xrange)
```

> 运行结果
```
测试range
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
测试xrange
xrange(10)
```

* 而使用for循环输出结果一致
```
for i in range(3):
    print(i)

for i in xrange(3):
    print(i)
```

> 输出结果
```
0
1
2
0
1
2
```


