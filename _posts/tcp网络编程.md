---
title: tcp网络编程
date: 2019-09-16 21:12:17
tags:
- python网络编程
categories:
- python
description: tcp可以类比于打电话的方式，在通讯之前需要先建立连接，较udp更为安全
---

[TCP百度百科](https://baike.baidu.com/item/TCP/33012?fr=aladdin)

<!--more-->
## 简介
* TCP通信需要经过**创建链接，数据传送，终止连接**三个步骤
* 通信双方都必须先建立连接才能进行通讯，双方都要为连接分配必要的系统内核资源，以管理连接的状态和传输
* TCP的连接是一对一的，因此，**基于广播的应用程序应使用UDP**，
