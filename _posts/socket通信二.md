---
title: socket通信二
date: 2019-11-14 18:56:20
tags:
- socket
categories:
- C/C++
description:
- C在socket编程中需要注意的地方
---

<!--more-->
### C语言socket编程说明
#### 服务端程序绑定地址
一般情况下，服务器有多个网卡，多个IP地址，socket通信可以指定用哪个地址来进行通信，对于程序员有两种选择
* 任意IP地址
```C
m_servaddr.sin_addr.s_addr = htonl(INADDR_ANY);  //本主机的任意IP地址
```

* 指定IP地址
```C
m_servaddr.sin_addr.s_addr = inet_addr("100.100.100.100");  //指定IP地址
```

#### 绑定通信端口
```C
m_servaddr.sin_port = htons(6789);  //绑定端口
```

#### 客户端程序指定服务端的IP地址
```C
struct hostent* hptr;  //IP地址的结构体指针
if((hptr=gethostbyname("100.100.100.100"))==0)
{
    perror("gethostbyname");
    close(sockfd);  //sockfd指客户端的套接字
    return -1;
}
```

#### send函数
send函数用于把数据通过socket发送给对端。不论是客户端还是服务端，应用程序都用send函数来向TCP连接的另一端发送数据。客户程序一般用send函数向服务器发送请求，而服务器则通常用send函数来向客户程序发送应答。
* 包含头文件
```C
#include <sys/types.h>
#include <sys/socket.h>
```

* 函数声明
```C
ssize_t send (int sockfd, const void *buf, size_t len, int flags);
```

sockfd为已建立好连接的socket。
buf为需要发送的数据的内存地址，可以是C语言基本数据类型变量的地址，也可以数组、结构体、字符串，内存中有什么就发送什么。
len需要发送的数据的长度，为buf中有效数据的长度。
flags填0, 其他数值意义不大。
函数返回已发送的字符数。出错时返回-1，错误信息errno被标记。
注意，就算是网络断开，或socket已被对端关闭，send函数不会立即报错，要过几秒才会报错。
如果send函数返回的错误（<=0），表示通信链路已不可用。

#### recv函数
recv函数用于接收对端socket发送过来的数据。
* 包含头文件
```C
#include <sys/types.h>
#include <sys/socket.h>
```

* 函数声明
```C
ssize_t recv (int sockfd, void *buf, size_t size, int flags);
```

sockfd为已建立好连接的socket。
buf为用于接收数据的内存地址，可以是C语言基本数据类型变量的地址，也可以数组、结构体、字符串，只要是一块内存就行了。
len需要接收数据的长度，不能超过buf的大小，否则内存溢出。
flags填0, 其他数值意义不大。
如果socket的对端没有发送数据，recv函数就会等待，如果对端发送了数据，函数返回接收到的字符数。出错时返回-1，错误信息errno被标记。如果socket被对端关闭，返回值为0。
如果recv函数返回的错误（<=0），表示通信链路已不可用。

#### 服务端有多个socket
对服务端来说，有多个socket，一个是用于监听的socket，还有一个就是客户端连接成功后，由**accept函数创建的**用于与客户端收发报文的socket。如果有多个客户端连接，就会有多个socket。

#### 程序退出时先关闭socket
socket是系统资源，操作系统打开的socket数量是有限的，在程序退出之前必须关闭已打开的socket，就像关闭文件指针一样，就像delete已分配的内存一样，极其重要。
值得注意的是，关闭socket的代码不能只在main函数的最后，那是程序运行的理想状态，还应该在main函数的每个return之前关闭。

### 相关的库函数
#### socket函数
socket函数用于创建一个新的socket，也就是向系统申请一个socket资源。socket函数用户客户端和服务端。
* 函数声明：
```C
int socket(int domain, int type, int protocol);
```

参数说明：

domain：协议域，又称协议族（family）。常用的协议族有AF_INET、AF_INET6、AF_LOCAL（或称AF_UNIX，Unix域Socket）、AF_ROUTE等。协议族决定了socket的地址类型，在通信中必须采用对应的地址，如AF_INET决定了要用ipv4地址（32位的）与端口号（16位的）的组合、AF_UNIX决定了要用一个绝对路径名作为地址。

type：指定socket类型。常用的socket类型有SOCK_STREAM、SOCK_DGRAM、SOCK_RAW、SOCK_PACKET、SOCK_SEQPACKET等。流式socket（SOCK_STREAM）是一种面向连接的socket，针对于面向连接的TCP服务应用。数据报式socket（SOCK_DGRAM）是一种无连接的socket，对应于无连接的UDP服务应用。

protocol：指定协议。常用协议有IPPROTO_TCP、IPPROTO_UDP、IPPROTO_STCP、IPPROTO_TIPC等，分别对应TCP传输协议、UDP传输协议、STCP传输协议、TIPC传输协议。

> 说了一大堆废话，第一个参数只能填AF_INET，第二个参数只能填SOCK_STREAM，第三个参数只能填0。

除非系统资料耗尽，socket函数一般不会返回失败。

#### gethostbyname函数
把ip地址或域名转换为hostent 结构体表达的地址。
* 函数声明：
```C
struct hostent *gethostbyname(const char *name);
```

参数name，域名或者主机名，例如"192.168.1.3"、"www.google.com"等。
返回值：如果成功，返回一个hostent结构指针，失败返回NULL。
gethostbyname只用于客户端。

gethostbyname只是把字符串的ip地址转换为结构体的ip地址，只要地址格式没错，一般不会返回错误。函数失败不会设置errno的值。

#### connect函数
向服务器发起连接请求。
* 函数声明：
```C
int connect(int sockfd, struct sockaddr * serv_addr, int addrlen);
```

函数说明：connect函数用于将参数sockfd 的socket 连至参数serv_addr 指定的服务端，参数addrlen为sockaddr的结构长度。
返回值：成功则返回0, 失败返回-1, 错误原因存于errno 中。
connect函数只用于客户端。
如果服务端的地址错了，或端口错了，或服务端没有启动，connect一定会失败。

#### bind函数
服务端把用于通信的地址和端口绑定到socket上。
* 函数声明：
```C
int bind(int sockfd, const struct sockaddr *addr,socklen_t addrlen);
```

参数sockfd，需要绑定的socket。
参数addr，存放了服务端用于通信的地址和端口。
参数addrlen表示addr结构体的大小。
如果绑定的地址错误，或端口已被占用，bind函数一定会报错，否则一般不会返回错误。

#### listen函数
listen函数把主动连接套接口变为被连接套接口，使得这个socket可以接受其它socket的连接请求，从而成为一个服务端的socket。

* 函数声明：
```C
int listen(int sockfd, int backlog)
```

返回：0──成功， -1──失败

参数sockfd是已经被bind过的套接字。socket函数返回的套接字是一个主动连接的套接字，在服务端的编程中，程序员希望这个套接字可以接受外来的连接请求，也就是被动等待客户端来连接。由于系统默认时认为一个套接字是主动连接的，所以需要通过某种方式来告诉系统，程序员通过调用listen函数来完成这件事。

参数backlog，这个参数涉及到一些网络的细节，比较麻烦，填5、10都行，一般不超过30。
当调用listen之后，服务端的套接字就可以调用accept来接受客户端的连接请求。
listen函数一般不会返回错误。

#### accept函数
服务端接受客户端的连接。
* 函数声明：
```C
int accept(int sockfd,struct sockaddr *addr,socklen_t *addrlen);
```

参数sockfd是已经被listen过的套接字。
参数addr用于存放客户端的地址信息，用sockaddr结构体表达，如果不需要客户端的地址，可以填0。
参数addrlen用于存放addr参数的长度，如果addr为0，addrlen也填0。
accept函数等待客户端的连接，如果没有客户端连上来，它就一直等待，这种方式称之为**阻塞**。
accept等待到客户端的连接后，创建**一个新的套接字**，函数返回值就是这个新的套接字，服务端使用这个新的套接字和客户端进行报文的收发。
accept在等待的过程中，如果被中断或其它的原因，函数返回-1，表示失败，如果失败，可以重新accept。

### 函数小结
服务端函数调用的流程是：socket->bind->listen->accept->recv/send->close
客户端函数调用的流程是：socket->connect->send/recv->close
