---
title: 利用tcp下载文件
date: 2019-09-19 20:28:48
tags:
- tcp
- python网络编程
categories:
- python
description: 使用python中的socket模块写一个客户端与服务器通讯下载文件的案例
---

<!--more-->

## 客户端
* 即要接收文件的一方
```py
from socket import *


def main():
    # 创建套接字
    tcp_socket = socket(AF_INET, SOCK_STREAM)

    # 获取服务器的ip port
    dest_ip = input("请输入对方的ip：")
    dest_port = input("请输入对方的port：")

    # 连接服务器
    tcp_socket.connect((dest_ip, int(dest_port)))

    # 获取下载的文件名称
    download_file_name = input("请输入要下载的文件名：")

    # 将文件名字发送到服务器
    tcp_socket.send(download_file_name.encode("utf-8"))

    # 接收文件数据
    recv_data = tcp_socket.recv(1024*1024)  # 1k*1024=1M

    # 保存接收到的数据到一个文件中，如果内容为空则不创建文件
    if recv_data:
        with open ("[new]"+download_file_name, "wb") as d_file:
            d_file.write(recv_data)

    # 关闭套接字
    tcp_socket.close()


if __name__ == "__main__":
    main()
```

## 服务器
```py
from socket import *


def send_file_to_client(client_socket, client_addr):
    # 接收客户端要下载的文件名
    file_name = client_socket.recv(1024).decode("utf-8")
    print("客户端{0}要下载的文件是{1}".format(client_addr, file_name))

    # 打开文件读取数据
    file_content = None
    try:
        dl_file = open(file_name, "rb")
        file_content = dl_file.read()
        dl_file.close()
    except Exception as e:
        print("没有该文件(%s)" % file_name)

    # 如果recv解堵塞，有两种方式：1.客户端发送了数据，2.客户端调用了close
    if file_content:
        # 回送数据
        client_socket.send(file_content)


def main():
    # 创建套接字
    tcp_serve_socket = socket(AF_INET, SOCK_STREAM)

    # 绑定本地信息
    tcp_serve_socket.bind(("", 7788))

    # 让默认套接字由主动变为被动,listen
    tcp_serve_socket.listen(128)

    while True:
        # 等待客户端的连接accept,未连接到客户端会进入堵塞
        print("等待客户端连接...")
        client_socket, client_addr = tcp_serve_socket.accept()
        
        # 发送文件
        send_file_to_client(client_socket, client_addr)
        # 关闭套接字
        client_socket.close()
        print("已为客户服务完成")

    tcp_serve_socket.close()


if __name__ == "__main__":
    main()
```

## 执行程序
### 准备工作
* 首先先在当前目录下新建一个文件，以供调试使用，假设新建一个名为`test.txt`的文件，并在里面写下一些内容，如
```
this is some text
这是一写文本
```

* 查看自己的ip地址
```bash
# 一般使用
ifconfig | grep inet
# archlinux使用
ip addr | grep inet
```

> 记住ip地址，假设为`123.12.10.154`

### 同时运行两端代码
* 客户端依次输入ip地址，端口号(程序中指定了7788，可更改为1024--65535之间的其他数字)
```
请输入对方的ip：123.12.10.154
请输入对方的port：7788
请输入要下载的文件名：test.txt
```

* 服务器中自动打印出了如下信息
```
等待客户端连接...
客户端('10.128.150.40', 34878)要下载的文件是test.txt
已为客户服务完成
等待客户端连接...
```

### 运行结果
打开文件管理器，发现在当前目录下出现了一个名为`[new]test.txt`的文件，打开，查看内容，发现与`test.txt`中内容一致，说明代码执行成功
