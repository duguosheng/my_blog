---
title: python函数嵌套
date: 2019-09-17 13:29:10
tags:
- python基础
categories:
- python
description: 在函数内部定义的函数，也叫内部函数
---

<!--more-->
## 使用
* 封装——数据隐藏,外部无法访问**嵌套函数**
* 提高代码复用率

## 示例
```py
def f1():
    print("f1")

    def f2():
        print("f2")

    f2()

f1()
# f2() 不可在外部调用
```

## nonlocal关键字
* **global**声明全局变量
* **ninlocal**声明外部变量


```py
def outer():
    b = 10

    def inner():
        nonlocal b
        b = 20

    inner()
    print(b)

outer()
```

> 结果
```
20
```


