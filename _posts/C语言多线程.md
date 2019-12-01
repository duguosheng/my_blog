---
title: C语言多线程
date: 2019-11-21 22:00:57
tags:
- 线程
categories:
- C/C++
---

修改自[码农有道的博客](https://blog.csdn.net/wucz122140729/article/details/98588567)

## 线程的概念
和多进程相比，多线程是一种比较节省资源的多任务操作方式。启动一个新的进程必须分配给它独立的地址空间，每个进程都有自己的堆栈段和数据段，系统开销比较高，进行数据的传递只能通过进行间通信的方式进行。在同一个进程中，可以运行多个线程，运行于同一个进程中的多个线程，它们彼此之间使用相同的地址空间，共享全局数据，启动一个线程所消耗的资源比启动一个进程所消耗的资源要少。
<!--more-->

## 线程的使用
### 创建线程
在Linux下，采用pthread_create函数来创建一个新的线程，函数声明：
* 包含头文件：
```C
#include <pthread.h>
```

* 函数声明：
```C
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,void *(*start_routine) (void *), void *arg);
```

参数thread为为指向线程标识符的地址。
参数attr用于设置线程属性，一般为空，表示使用默认属性。
参数start_routine是线程运行函数的地址。
参数arg是线程运行函数的参数。新创建的线程从start_routine函数的地址开始运行，该函数只有一个无类型指针参数arg。  

> 在编译时注意加上-lpthread参数，以调用静态链接库。因为pthread并非Linux系统的默认库。

### 线程的终止
如果进程中的任一线程调用了exit(0)，则整个进程会终止，所以，在线程的start_routine函数中，不能采用exit。

* 线程的终止有三种方式：
1）线程的start_routine函数代码结束，自然消亡。
2）线程的start_routine函数调用pthread_exit结束，不想活了。
3）被主进程或其它线程中止，被杀了，一般情况不会这么做。

pthread_exit函数的声明如下：
```C
void pthread_exit(void *retval);
```

参数retval填空。

* 线程主函数的函数体中，不能使用return语句，如果想退出线程，可以用pthread_exit(0);返回。

## 线程同步
线程同步？怎么同步？一起运行？一起停止？我当年听说线程同步这个词的时候，也是一头雾水。

在人们的日常生活中，所说的锁大概有两种：一种是不允许访问；另一种是资源忙，同一时间只允许一个使用者占用，其它使用者如果要使用，必须要等待。

1）不允许访问的锁好理解，就像每户人家的锁，不允许外人进入。
2）第二种锁，例如火车上的厕所，它是公共的，空闲的时候任何人可以进入，人进去以后就会把它锁起来，其它的人如果要上厕所，必须等待解锁，即里面的人出来。还有红绿灯，红灯是加锁，绿灯是解锁。

对多线程来说，资源是共享的，基本上不存在不允许访问的情况，但是，共享的资源在某一时间点只能有一个线程占用，这种情况是存在的，所以需要给资源加锁。

线程的锁的种类有互斥锁、读写锁、条件变量、自旋锁、信号灯。

在本章节中，只介绍互斥锁，其它的锁应用场景极少

### 互斥锁
互斥锁机制是同一时刻只允许一个线程占有共享的资源。

* 初始化锁
```C
int pthread_mutex_init(pthread_mutex_t *mutex,const pthread_mutex_attr_t *mutexattr);
```

其中参数 mutexattr 用于指定锁的属性（见下），如果为NULL则使用缺省属性。
互斥锁的属性在创建锁的时候指定，当资源被某线程锁住的时候，其它的线程在试图加锁时表现将不同。当前有四个值可供选择：

1）PTHREAD_MUTEX_TIMED_NP，这是缺省值，也就是普通锁。当一个线程加锁以后，其余请求锁的线程将形成一个等待队列，并在解锁后按优先级获得锁。这种锁策略保证了资源分配的公平性。
2）PTHREAD_MUTEX_RECURSIVE_NP，嵌套锁，允许同一个线程对同一个锁成功获得多次，并通过多次unlock解锁。
3）PTHREAD_MUTEX_ERRORCHECK_NP，检错锁，如果同一个线程请求同一个锁，则返回EDEADLK，否则与PTHREAD_MUTEX_TIMED_NP类型动作相同。
4）PTHREAD_MUTEX_ADAPTIVE_NP，适应锁，动作最简单的锁类型，等待解锁后重新竞争。

* 阻塞加锁
```C
int pthread_mutex_lock(pthread_mutex *mutex);
```

* 非阻塞加锁
```C
int pthread_mutex_trylock( pthread_mutex_t *mutex);
```

该函数语义与 pthread_mutex_lock() 类似，不同的是在锁已经被占据时立即返回 EBUSY，不是挂起等待。

* 解锁（要求锁是lock状态，并且由加锁线程解锁）
```C
int pthread_mutex_unlock(pthread_mutex *mutex);
```

* 销毁锁（此时锁必需unlock状态，否则返回EBUSY）
```C
int pthread_mutex_destroy(pthread_mutex *mutex);
```

## 应用经验
Linux没有真正意义上的线程，它的实现是由进程来模拟，属于用户级线程。所以，在Linux系统下，进程与线程在性能和资源消耗方面没有本质的差别。对我们程序员来说，进程不能共享全局数据，线程可以共享全局数据，各位可以根据应用场景选择采用多进程或多线程。
