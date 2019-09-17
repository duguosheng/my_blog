---
title: 使用zip()并行迭代
date: 2019-09-17 10:29:54
tags:
- python基础
categories:
- python
description: zip()可以并行遍历多个元组，字典，
---

<!--more-->

* 实例
```py
names = ("张三", "李四", "王五", "赵六")
ages = (20, 30, 35, 28)
jobs = ("老师", "工程师", "程序员")

for name, age, job in zip(names, ages, jobs):
    print("{0}-{1}-{2}".format(name, age, job))
```

* 结果
```
张三-20-老师
李四-30-工程师
王五-35-程序员
```

> 当示例中一个元组`job`遍历结束后整个遍历就结束了，因而没有输出`赵六`的信息

* 不使用`zip()`实现上述功能的代码
```py
for i in range(3)
    print("{0}-{1}-{2}".format(names[i], ages[i], jobs[i]))
```


