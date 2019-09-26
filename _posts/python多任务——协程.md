---
title: python多任务——协程
date: 2019-09-23 19:11:02
tags:
- python多任务
categories:
- python
description: 协程是基于单线程来实现并发，即只用一个主线程情况下实现并发，是由用户控制的
---

<!--more-->

## 创建协程
### 使用`yield`和`next()`创建协程
* 关于[yield和next()](http://www.duguosheng.xyz/2019/09/23/迭代器与生成器/)
```py
import time



def task1():
    while True:
        print("----1----")
        time.sleep(0.1)
        yield


def task2():
    while True:
        print("****2****")
        time.sleep(0.1)
        yield


while True:
    next(task1())
    next(task2())
```

> 运行结果
```
----1----
****2****
----1----
****2****
----1----
****2****
   ...
   ...
```

### 使用`greenlet`
* 如果没有可以使用`pip3`安装`greenlet`
```bash
sudo pip3 install greenlet
```

* `greenlet`对于`yield`进行了封装，使用户可以按照正常写函数的方式实现多任务
* 使用：
  * `gr = greenlet(目标函数)`创建对象
  * `gr.switch()`切换执行任务
* 示例
```py
import time
import greenlet


def task1():
    while True:
        print("----1----")
        # 切换到task2
        gr2.switch()
        time.sleep(0.1)


def task2():
    while True:
        print("****2****")
        # 切换到task1
        gr1.switch()
        time.sleep(0.1)


gr1 = greenlet.greenlet(task1)
gr2 = greenlet.greenlet(task2)

# 切换到task1
gr1.switch()
```

### 使用gevent
* 如果没有可以使用下面的命令安装
```bash
sudo pip3 install gevent
```

> `gevent`是在`greenlet`的基础上再次封装的

* `gevent`遇到延时操作自动切换任务，这个延时操作不能是`time.sleep()`而需要是`gevent.sleep()`
* 代码示例
```py
import gevent


def fun(n):
    for i in range(n):
        # 获取当前协程id
        print(gevent.getcurrent(), i)
        gevent.sleep(0.01)


g1 = gevent.spawn(fun, 3)
g2 = gevent.spawn(fun, 3)
g1.join()
g2.join()
```

> 运行结果
```
<Greenlet at 0x7f92a16fb710: fun(3)> 0
<Greenlet at 0x7f92a16fb830: fun(3)> 0
<Greenlet at 0x7f92a16fb710: fun(3)> 1
<Greenlet at 0x7f92a16fb830: fun(3)> 1
<Greenlet at 0x7f92a16fb710: fun(3)> 2
<Greenlet at 0x7f92a16fb830: fun(3)> 2
```

> 可以发现三者交替执行，实现了多任务

* 上述创建协程方法有些麻烦，还可以使用`joinall([任务1, 任务2, ...])`的方式，它的参数是一个列表
```py
gevent.joinall([gevent.spawn(fun, 3),
                gevent.spawn(fun, 3)])
```


### 如果原来的程序中使用了`time.sleep()`进行延时
```py
import gevent
import time


def fun(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.01)


g1 = gevent.spawn(fun, 3)
g2 = gevent.spawn(fun, 3)
g1.join()
g2.join()
```

> 运行结果
```
<Greenlet at 0x7fe70455e710: fun(3)> 0
<Greenlet at 0x7fe70455e710: fun(3)> 1
<Greenlet at 0x7fe70455e710: fun(3)> 2
<Greenlet at 0x7fe70455e830: fun(3)> 0
<Greenlet at 0x7fe70455e830: fun(3)> 1
<Greenlet at 0x7fe70455e830: fun(3)> 2
```

> 可以看出，两个任务并没有交替执行，而是一个执行完了另一个再执行，并没有实现多任务

#### 解决方法
* 在执行前之前写如下代码，使用patch_all()解决
```py
from gevent import monkey
monkey.patch_all()
```

