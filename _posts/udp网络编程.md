---
title: udp网络编程
date: 2019-09-15 20:33:13
tags:
- udp
- python网络编程
categories:
- python
description: udp可以类比写信的模式，在通信之前不需要建立连接
---

[UDP百度百科](https://baike.baidu.com/item/UDP/33012?fr=aladdin)

<!--more-->

## 创建udp套接字
```py
import socket

udp_socket = socket.socket(socket.AF_INET, socket.DGRAM)
```

## 发送数据
使用`sendto()`方法
```py
send_data = input("please input:")
# 发送的数据(utf-8编码)  (ip地址, 端口)
udp_socket.sendto(send_data.encode("utf-8"), ("X.X.X.X", port))  # 如果发送方未绑定端口，系统会随机分配1024-65535
```

## 获取端口
```python
# 创建元组，ip地址和端口号
local_addr = ("", 7788)    # ip地址一般不用写，表示本机的任意一个ip
# 绑定
udp_socket.bind(local_addr)
```

* 程序实例
```py
import socket


def send_msg(udp_socket):
    """发送数据"""
    dest_ip = input("请输入对方的ip：")
    dest_port = input("请输入对方的port：")
    send_data = input("请输入发送的消息：")
    udp_socket.sendto(send_data.encode("utf-8"), (dest_ip, int(dest_port)))


def recv_msg(udp_socket):
    """接收数据"""
    # 接收数据,1024是本次接收的最大字节数，如果未接收到会进入阻塞状态
    recv_data = udp_socket.recvfrom(1024)  # 会接收到数据和发送方的ip地址和端口
    recv_msge = recv_data[0]
    send_addr = recv_data[1]

    # 打印接收到的数据, 需要指定解码，如果是windows发送，应当使用gbk解码
    print("接收到的数据是：%s 来源于：%s" % (recv_msge.decode("utf-8"), str(send_addr)))


def main():
    # 创建套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 绑定信息
    udp_socket.bind(("", 7788))

    while True:
        print("0退出，1发送，2接收")
        option = input("请输入指令：")
        if option == "0":
            break
        elif option == "1":
            # 发送
            send_msg(udp_socket)
        elif option == "2":
            # 接收并显示
            recv_msg(udp_socket)
        else:
            print("输入错误，请重新输入")


if __name__ == "__main__":
    main()
```

> `socket`套接字是**全双工**工作模式，即可以同时收发



