---
title: python多任务——线程
date: 2019-09-19 21:28:20
tags:
- python多任务
categories:
- python
description: 线程（英语：thread）是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。
---

<!--more-->

## Thread创建线程
### 使用Threading模块
* 语法格式
```py
# 导入threading模块
import threading

# 创建线程
my_thread = threading.Thread(target=函数名)
# 开启线程
my_thread.start()
```

* 代码示例
```py
import threading
import time


def testThread():
    while True:
        print("hello")
        time.sleep(1)


def main():
    thrd = threading.Thread(target=testThread)
    thrd.start()


if __name__ == "__main__":
    main()
```

> 运行结果： 每隔一秒打印一次hello

### 继承Thread类创建线程
* 步骤
  * 创建类，继承Thread类
  * 在这个类中重写`run()`方法
* 示例
```py
import time
from threading import *


class MyThread(Thread):
    """继承Thread类"""
    def run(self):
        """重写run()方法"""
        for i in range(3):
            time.sleep(1)
            print("hello")


def main():
    mythread = MyThread()
    # 调用start()方法后会自动调用run()方法
    mythread.start()


if __name__ == "__main__":
    main()
```

> 执行结果：每隔一秒打印一次hello

## 查看线程数量
* 使用Threading模块中的`enumerate()`方法
```py
print(threading.enumerate())
```

* 示例
```py
import threading
import time


def testThread1():
    for i in range(3):
        print("thread---1")
        time.sleep(0.1)


def testThread2():
    for i in range(3):
        print("thread---2")
        time.sleep(0.1)


def main():
    thrd1 = threading.Thread(target=testThread1)
    thrd2 = threading.Thread(target=testThread2)
    thrd1.start()
    thrd2.start()
    print(threading.enumerate())

    time.sleep(1)
    print(threading.enumerate())


if __name__ == "__main__":
    main()
```

> 运行结果
```
thread---1
thread---2
[<_MainThread(MainThread, started 140351833364096)>, <Thread(Thread-1, sta
rted 140351824549632)>, <Thread(Thread-2, started 140351746602752)>]
thread---1
thread---2
thread---1
thread---2
[<_MainThread(MainThread, started 140351833364096)>]
```

> 注意： 只有调用了`start()`方法时才会创建线程并开始执行，而使用`Thread(target=)`只是创建一个对象，所以只有放在`start()`后和线程结束前才可以使用`enumerate()`查看

* 由运行结果可见，当一个线程结束后，不会再打印它的信息，即列表中不再存在，由此可以判断其他线程是否都结束
```py
# 当只有主线程的时候
if len(threading.enumerate()) == 1:
    break
```

## 线程中传递参数args
* 线程中传递的参数使用args指定，必须是一个元组
```py
import threading
import time


g_list1 = [11, 22]
g_list2 = [11, 22]


def test1(list1):
    list1.append(33)
    print("在test1中，list1=%s" % list1)


def test2(list1, list2):
    list1.append(44)
    list2.append(33)
    print("在test2中，list1=%s" % list1)
    print("在test2中，list2=%s" % list2)


def main():
    # target指定函数地址
    # args指定参数，必须是一个元组
    t1 = threading.Thread(target=test1, args=(g_list1, ))  # 当参数只有一个时，使用args=(temp, )的方式
    t2 = threading.Thread(target=test2, args=(g_list1, g_list2))
    t1.start()
    time.sleep(0.1)
    t2.start()
    time.sleep(0.1)


if __name__ == "__main__":
    main()
```

> 运行结果
```
在test1中，list1=[11, 22, 33]
在test2中，list1=[11, 22, 33, 44]
在test2中，list2=[11, 22, 33]
```

## 互斥锁解决资源竞争
### 资源竞争

* 由于多线程共用资源，会产生资源竞争的问题，如下面的代码，让两个线程同时对全局变量执行加一的操作，若不存在资源竞争，应当结果为二者循环次数之和，而结果往往小于此
* 这是因为当一个线程拿到数据加一后，可能还未存放结果，这个数据就被另一个线程调用，这样就会使数值低于预期
```py
import time
import threading
g_num = 0


def add_1(times):
    global g_num
    for i in range(times):
        g_num += 1
    print("thread2-----%d" % g_num)


def add_2(times):
    global g_num
    for i in range(times):
        g_num += 1
    print("thread2-----%d" % g_num)


def main():
    t1 = threading.Thread(target=add_1, args=(1000000, ))
    t2 = threading.Thread(target=add_2, args=(1000000, ))
    t1.start()
    t2.start()
    while True:
        # 确保执行完毕,每隔1s判断一次
        time.sleep(1)
        if len(threading.enumerate()) == 1:
            print("main-----%d" % g_num)
            break


if __name__ == "__main__":
    main()
```

> 运行结果
```py
thread2-----1197292
thread2-----1253537
main-----1253537    # 这个结果不一定，但往往小于预期的2000000
```

### 互斥锁
* 当对象被线程调用时给对象上锁，只有使用完成后才释放，保证同一资源只有一个线程在调用
```py
# 创建锁，默认未锁定
mutex = threading.Lock()

# 锁定
mutex.acquire()

# 释放
mutex.release()
```

* 以上例中的`add_1()`为例
```py
# 创建互斥锁
mutex = threading.Lock()


def add_1(times):
    global g_num
    mutex.acquire()
    for i in range(times):
        g_num += 1
    mutex.release()
    print("thread1-----%d" % g_num)

# add_2()改变的格式同add_1()
```

> 执行结果
```py
thread1-----1000000
thread2-----2000000
main-----2000000
```

* 上面的改动中将锁放在了循环之外，因而当一个线程抢到资源后后全部执行结束后才释放，在实际开发中，应当尽量减小锁的范围，本例中可将锁置于循环之中
```py
def add_1(times):
    global g_num
    for i in range(times):
        mutex.acquire()
        g_num += 1
        mutex.release()
    print("thread1-----%d" % g_num)
```

> 执行结果
```py
thread1-----1971999     # 两个线程交替拿到资源
thread2-----2000000
main-----2000000
```

### 死锁
* 死锁是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，如
```py
mutexA = threading.Lock()
mutexB = threading.Lock()

def test1():
    mutexA.acquire()
    time.sleep(1)
    mutexB.acquire()
    mutexA.release()


def test2():
    mutexB.acquire()
    time.sleep(1)
    mutexA.acquire()
    mutexB.release()
```

> 上例中，`test1()`先将`mutexA`上锁，`test2()`先将`mutexB`上锁
> 当休眠结束后，`test1()`要给`mutexB`上锁，之后才能释放`mutexA`，而`mutexB`已经被`test2()`上锁占用，因而要等待`test2()`释放`mutexB`
> 而`test2()`要释放`mutexB`，又要等待`test1()`释放`mutexA`，这样两者都不会进行到下一步，从而产生了**死锁** 


* 避免：
  * 程序设计中避免
  * 添加超时算法等





