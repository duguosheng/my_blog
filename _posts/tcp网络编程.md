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

## TCP客户端
* TCP严格区分服务器和客户端，服务器就是提供服务的一方，客户端就是需要服务的一方，而UDP不区分

### TCP客户端构建流程
* 代码流程
```py
from socket import *


def main():
    # 创建tcp的套接字
    tcp_client_socket = socket(AF_INET, SOCK_STREAM)

    # 连接服务器
    serve_ip = input("请输入服务器ip：")
    serve_port = input("请输入服务器port：")
    serve_addr = (serve_ip, int(serve_port))
    tcp_client_socket.connect(serve_addr)

    # 发送/接收数据,使用send方法，不需要多次指定套接字
    send_data = input("请输入要发送的数据：")
    tcp_client_socket.send(send_data.encode("gbk"))

    # 关闭套接字
    tcp_client_socket.close()


if __name__ == "__main__":
    main()
```

## TCP服务器
```py
from socket import *


def main():
    # 创建套接字
    tcp_serve_socket = socket(AF_INET, SOCK_STREAM)

    # 绑定本地信息
    tcp_serve_socket.bind(("", 7788))

    # 让默认套接字由主动变为被动,listen
    # 使用socket创建的套接字默认的属性是主动的，使用listen可将其变为被动，这样就可以接收别人的连接了
    tcp_serve_socket.listen(128)

    # 循环为多个客户端服务
    while True:
        # 等待客户端的连接accept,未连接到客户端会进入堵塞
        # 如果有新的客户端来连接服务器，那么就产生一个新的套接字专门为这个客户端服务
        # client_socket用来为这个客户端服务
        # tcp_serve_socket就可以省下来专门等待其他新的客户端的连接
        print("等待客户端连接...")
        client_socket, client_addr = tcp_serve_socket.accept()
        print("新的客户端已连接：%s" % str(client_addr))

        # 循环为同一个客户端服务
        while True:
            # 接收客户端请求,会进入堵塞
            recv_data = client_socket.recv(1024)
            print("客户端请求是：%s" % recv_data)

            # 如果recv解堵塞，有两种方式：1.客户端发送了数据，2.客户端调用了close
            if recv_data:
                # 回送数据
                client_socket.send("hello".encode("gbk"))
            else:
                break

        # 关闭套接字
        client_socket.close()
        print("已为客户服务完成")

    tcp_serve_socket.close()


if __name__ == "__main__":
    main()
```


