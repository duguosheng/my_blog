---
title: socket通信一
date: 2019-11-13 20:11:36
tags:
- socket
categories:
- C/C++
description:
- IP地址+端口组成socket套接字
---

<!--more-->

[一些基本概念](http://www.duguosheng.xyz/2019/09/12/%E7%BD%91%E7%BB%9C%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5/)

[C语言中需要注意的socket编程知识](http://www.duguosheng.xyz/2019/11/14/socket%E9%80%9A%E4%BF%A1%E4%BA%8C/)

* C语言实现socket通信
> `_public.h`
```C++
#ifndef __PUBLIC_H
#define __PUBLIC_H

#include <stdio.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <netdb.h>

#define IP_ADDR "100.100.100.100"  //这里填写本机IP
#define IP_PORT 7890  //指定端口


bool SocketClient();
bool SocketServe();

#endif
```

> `socket_serve.cpp`
这是服务器的程序，服务器的流程包括
* 服务端要先启动，准备好一个通信通道，表示可以在某地址和端口上可以接收客户连接。
* 等待客户端的连接请求。
* 等待并接收客户端发过来的数据，处理该客户的数据，处理完成后，向客户端返回处理结果。
* 不断的重复第上一步，直到客户端断开连接。
* 关闭服务端
```C++
#include "_public.h"

int main()
{
    if(SocketServe()==false)
        return -1;
    return 0;
}

bool SocketServe()
{
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);  // 创建服务器用于监听的socket
    //把服务端用于通信的地址和端口绑定到socket上
    struct sockaddr_in servaddr;  //服务器地址信息的数据结构
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;  //协议族，在socket编程中只能是AF_INET
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);  //本主机的任意IP
    //servaddr.sin_addr.s_addr = inet_addr(IP_ADDR);  //绑定指定的地址
    servaddr.sin_port = htons(IP_PORT);  //绑定通信端口
    if(bind(listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr))!=0)
    {
        perror("bind");
        close(listenfd);
        return false;
    }
    //把socket设定为监听模式
    if(listen(listenfd, 5)!=0)
    {
        perror("listen");
        close(listenfd);
        return false;
    }

    //接收客户端的连接
    int clientfd;  //客户端的socket
    int socklen = sizeof(struct sockaddr_in);
    struct sockaddr_in clientaddr;  //客户端的地址信息
    clientfd = accept(listenfd, (struct sockaddr *)&clientaddr, (socklen_t *)&socklen);
    printf("客户端%s已连接\n", inet_ntoa(clientaddr.sin_addr));

    //与客户端通信，接收客户端发过来的报文后，回复OK
    char buffer[1024];
    while(true)
    {
        memset(buffer, 0, sizeof(buffer));
        if(recv(clientfd, buffer, sizeof(buffer), 0)<=0)
            break;
        printf("接收：%s\n", buffer);

        strcpy(buffer, "OK");
        if(send(clientfd, buffer, strlen(buffer), 0)<=0)
            break;
        printf("发送：%s\n", buffer);
    }

    close(listenfd);
    close(clientfd);
    return true;
}
```

> `serve_client.cpp`
这是客户端的程序，客户端的流程为
* 打开一个通信通道，连接到服务端已准备好的端口。
* 向服务端发送数据，等待并接收服务端处理结果。
* 不断的重复第上一步，直到全部的数据被处理完。
* 关闭与服务器的连接。
```C++
#include "_public.h"

int main()
{
    if(SocketClient()==false)
        return -1;
    return 0;
}

bool SocketClient()
{
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);  //创建客户端的socket

    struct hostent *hptr;  //ip地址的结构体
    if((hptr=gethostbyname(IP_ADDR))==0)
    {
        perror("gethostbyname");
        close(sockfd);
        return false;
    }
    
    //把服务器的地址和端口转换为结构体
    struct sockaddr_in servaddr;
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(IP_PORT);
    memcpy(&servaddr.sin_addr, hptr->h_addr, hptr->h_length);

    //项服务器发起连接请求
    if(connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr))!=0)
    {
        perror("connect");
        close(sockfd);
        return false;
    }

    char str_buffer[1024];

    //与服务段通讯，发送一个报文后等待回复，然后发送下一个报文
    for(int i=0; i<5; i++)
    {
        memset(str_buffer, 0, sizeof(str_buffer));
        sprintf(str_buffer, 0, sizeof(str_buffer));
        sprintf(str_buffer, "这是第%d个报文", i);
        if(send(sockfd, str_buffer, strlen(str_buffer), 0)<=0)
            break;
        printf("发送：%s\n", str_buffer);

        memset(str_buffer, 0, sizeof(str_buffer));
        if(recv(sockfd, str_buffer, sizeof(str_buffer), 0)<=0)
            break;
        printf("接收：%s\n", str_buffer);
    }
    close(sockfd);
    return true;
}
```

> 分别在两个终端窗口下运行代码即可看到效果
