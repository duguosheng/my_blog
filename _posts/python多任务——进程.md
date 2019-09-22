---
title: python多任务——进程
date: 2019-09-20 20:15:48
tags:
- python多任务
categories:
- python
description: 一个任务就是一个进程（Process），比如打开一个浏览器就是启动一个浏览器进程，一个进程包含一个或多个线程
---

<!--more-->

## 进程及其状态
### 进程
* 程序：例如`xxx.py`这是程序，是静态的
* 进程：程序运行起来后，**代码+用到的资源**称为进程

### 进程的状态
任务数往往大于CPU核数，因而一定有一些任务正在进行，另一些任务等待，这样就导致了进程有不同的状态
![进程的状态](pro_1.png)
* 就绪态：运行条件都满足，等待CPU调度
* 执行态：CPU正在执行
* 等待态：等待某些条件满足，例如一个程序`sleep`中

## 创建进程
### 使用`multiprocessing`模块
```py
# 导入模块
import multiprocessing
import time


def testProcess():
    for i in range(5):
        print("process------")
        time.sleep(1)


def main():
    # 创建进程
    my_pro = multiprocessing.Process(target=testProcess)
    # 启动进程
    my_pro.start()
    time.sleep(10)


if __name__ == "__main__":
    main()
```

## 进程间通信
> 线程间资源共享
> 进程间资源不共享，是独立的
* 举例说明
```py
import multiprocessing
import threading
import time


g_num = 0


def test1():
    global g_num
    for i in range(10):
        g_num += 1
    print("test1-----%d" % g_num)


def test2():
    global g_num
    for i in range(10):
        g_num += 1
    print("test2-----%d" % g_num)


def test_pro():
    print("测试进程")
    p1 = multiprocessing.Process(target=test1)
    p2 = multiprocessing.Process(target=test2)
    p1.start()
    time.sleep(0.01)
    p2.start()


def test_thr():
    print("测试线程")
    t1 = threading.Thread(target=test1)
    t2 = threading.Thread(target=test2)
    t1.start()
    time.sleep(0.01)
    t2.start()


def main():
    # test_pro()
    test_thr()


if __name__ == "__main__":
    main()
```

> 结果
```
测试线程
test1-----10
test2-----20
```

* 改变`main()`函数
```py
def main():
    test_pro()
    # test_thr()
```

> 再次执行
```
测试进程
test1-----10
test2-----10
```

* 实现进程间通信：**`Queue`**
> `Queue`是队列的意思，遵循先进先出的原则
> 一个进程向`Queue`中写入数据，另一个进程从`Queue`中取出，就可实现通信
> 这是一种比较低端的方式，还可使用`socket`，`redis`等实现
* 代码
```py
import multiprocessing
import time


def download_from_web(queue):
    """模拟从网上下载数据"""
    data = [11, 22, 33, 44]

    # 向对列中写入数据
    for temp in data:
        queue.put(temp)

    print("已全部存入队列")


def analysis_data(queue):
    """数据处理"""
    d_data = list()

    # 获取数据
    while not queue.empty():
        data = queue.get()
        d_data.append(data)

    print("接收完成--%s" % d_data)


def main():
    # 创建一个队列
    queue = multiprocessing.Queue()

    # 创建多个进程，将队列的引用当作实参传递到里面
    p1 = multiprocessing.Process(target=download_from_web, args=(queue, ))
    p2 = multiprocessing.Process(target=analysis_data, args=(queue, ))
    p1.start()
    time.sleep(0.01)
    p2.start()


if __name__ == "__main__":
    main()
```

> 运行结果
```
已全部存入队列
接收完成--[11, 22, 33, 44]
```

> 成功实现了进程间的通讯


## 进程池
> 进程不多时可以直接使用`Process`动态生成多个进程，但如果所需进程过多，手动创建进程工作量太大，且不利于cpu的运行，因而可以使用进程池`Pool`
> 进程池是资源进程、管理进程组成的技术的应用(引自[百度百科](https://baike.baidu.com/item/%E8%BF%9B%E7%A8%8B%E6%B1%A0/3765641?fr=aladdin))
>> 资源进程：预先创建好的空闲进程，管理进程会把工作分发到空闲进程来处理。
>> 管理进程：管理进程负责创建资源进程，把工作交给空闲资源进程处理，回收已经处理完工作的资源进程。
> 这样进程使有进有出，有序管理
* 代码
```py
# 导包
from multiprocessing import Pool

# 定义进程池
po = Pool(3)    # 3指最多同时执行3个进程

# 添加进程队列，target指进程地址，args是进程函数的参数，是一个元组
po.apply_asyns(target, args)

# 关闭进程池
po.close()

# 等待po中所有子进程完成，必须在close()语句之后，没有这句代码，可以使主进程先结束，子进程也无法继续进行
po.join()
```

* 案例
```py
from multiprocessing import Pool
import os
import time
import random


def worker(msg):
    t_start = time.time()
    print("{0}开始执行，进程号{1}".format(msg, os.getpid()))
    time.sleep(random.random())
    t_stop = time.time()
    print(msg, "执行完毕，耗时%f" % (t_stop-t_start))


def main():
    po = Pool(3)
    for i in range(10):
        po.apply_async(worker, (i+1, ))

    print("---start---")
    po.close()
    po.join()
    print("----end----")


if __name__ == "__main__":
    main()
```

> 运行结果
```
---start---
1开始执行，进程号22044
2开始执行，进程号22045
3开始执行，进程号22046
1 执行完毕，耗时0.175232
4开始执行，进程号22044
3 执行完毕，耗时0.217286
5开始执行，进程号22046
5 执行完毕，耗时0.187288
6开始执行，进程号22046
4 执行完毕，耗时0.613512
7开始执行，进程号22044
2 执行完毕，耗时0.860506
8开始执行，进程号22045
7 执行完毕，耗时0.470875
9开始执行，进程号22044
6 执行完毕，耗时0.969476
10开始执行，进程号22046
9 执行完毕，耗时0.312829
8 执行完毕，耗时0.909825
10 执行完毕，耗时0.897377
----end----
```

> 可以从进程号看出一直在重复利用`22044, 22045, 22046`三个pid
> 从输出来看，可以看出来同一时间最多只有三个进程执行

* 注意：**在进程池间使用队列通信，不能使用`multiprocessing.Queue()`，而要使用`multiprocessing.Manager().Queue()`**

## 综合案例
* 实现文件复制功能
```py
import os
import multiprocessing


def copy_file(queue, file_name, src_dir, dest_dir):
    src_file = open(src_dir+"/"+file_name, "rb")
    # print("正在复制%s" % src_file)
    content = src_file.read()
    dest_file = open(dest_dir+"/"+file_name, "wb")
    dest_file.write(content)

    # 如果拷贝完了文件，就向队列写入数据，表示下载完成
    queue.put(file_name)


def main():
    # 获取用户需要copy的文件名
    src_dir = input("请输入要复制的文件夹：")

    # 创建一个新的文件夹
    try:
        dest_dir = src_dir+"_copy"
        os.mkdir(dest_dir)
    except:
        print("该文件夹已存在")

    # 获取文件夹中所有的待copy的文件名字listdir()
    file_names = os.listdir(src_dir)
    total_num = len(file_names)
    current_num = 0

    # 创建进程池
    po = multiprocessing.Pool(5)

    # 创建一个队列
    queue = multiprocessing.Manager().Queue()

    # 向进程池添加复制任务
    for file_name in file_names:
        po.apply_async(copy_file, (queue, file_name, src_dir, dest_dir))

    po.close()
    # po.join()
    # 显示执行进度
    while True:
        file_name = queue.get()
        current_num += 1
        now_step = current_num/total_num
        print("\r已拷贝%s-------进度%.2f %%              " % (file_name, 100*now_step), end="")
        if now_step == 1:
            break

    print()


if __name__ == "__main__":
    main()
```



