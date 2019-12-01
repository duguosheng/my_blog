---
title: 关于sockaddr与sockaddr_in详解
date: 2019-11-15 16:54:06
tags:
- 网络编程
categories:
- C/C++
description:
- struct sockaddr 和 struct sockaddr_in 这两个结构体用来处理网络通信的地址
---

[详解](https://blog.csdn.net/qingzhuyuxian/article/details/79736821)

总的来说就是后者弥补了前者的一些缺陷，但是由于某些函数需要传入sockaddr类型的数据，可以用sockaddr_in赋值后，强转为sockaddr作为参数。
