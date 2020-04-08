---
title: fcntl函数及示例解析
date: 2020-03-28 11:46:39
tags:
- fcntl
categories:
- unix环境高级编程
description:
- fcntl可以改变已经打开文件的属性
---

<!--more-->

### 函数原型
```c
#include <fcntl.h>
int fcntl(int fd, int cmd, ... /*int arg*/);
```

> `fd`:文件描述符 cmd:影响该函数的操作命令
* 该函数具有以下五种功能
> 复制一个已有的文件描述符(cmd=F_DUPFD或F_DUPFD_CLOEXEC)
> 获取/设置文件描述符标志(cmd=F_GETFD或F_SETFD)
> 获取/设置文件状态标志(cmd=F_GETFL或F_SETFL)
> 获取/设置异步IO所有权(cmd=F_GETOWN或F_SETOWN)
> 获取/设置记录锁(cmd=F_GETLK，F_SETLK或F_SETLKW)

### 例程解析
关于cmd参数的详细内容参考[csdn博客](https://blog.csdn.net/qq_37414405/article/details/83690447)
这里主要讲《UNIX环境高级编程》第三版67页的例程
```C
#include <fcntl.h>
#include "apue.h"
int main(int argc, char* argv[]) {
    int val;
    if (argc != 2)
        err_quit("usage: p67 <descriptor#>");
    if ((val = fcntl(atoi(argv[1]), F_GETFL, 0)) < 0)
        err_sys("fcntl error for fd %d", atoi(argv[1]));
    //由于历史原因，这三种标志并不各占一位，而是0,1,2,所以需要先用O_ACCMODE(0003)取得访问方式位
    switch (val & O_ACCMODE) {
        case O_RDONLY:
            printf("read only");
            break;
        case O_WRONLY:
            printf("write only");
            break;
        case O_RDWR:
            printf("read write");
            break;
        default:
            err_dump("unknown access mode");
    }
    if (val & O_APPEND)
        printf(", append");
    if (val & O_NONBLOCK)
        printf(", nonblocking");
    if (val & O_SYNC)
        printf(", synchronous writes");
#if !defined(_POSIX_C_SOURCE) && defined(O_FSYNC) && (O_FSYNC != O_SYNC)
    if (val & O_FSYNC)
        printf(", synchronous writes");
#endif
    putchar('\n');
    exit(0);
}
```

假设编译出的可执行文件为`a.out`，则在终端下执行命令及其结果为
```bash
$ ./a.out 0 < /dev/tty
read only
$ ./a.out 1 > temp.foo
$ cat temp.foo
write only
$ ./a.out 2 2>>temp.foo
write only, append
$ ./a.out 5 5<>temp.foo
read write
```

首先解释程序中`F_GETFL`的作用：当使用此参数作为fcntl的cmd参数传入时，对应于fd的文件状态标志作为函数值返回
* 关于unix中的重定向[重定向说明](http://www.duguosheng.xyz/2020/03/27/unix%E4%B8%8B%E7%9A%84%E9%87%8D%E5%AE%9A%E5%90%91/#more)

* 依次解析
第一行命令`./a.out 0 < /dev/tty`，则C语言程序中argv[0]="./a.out"，argv[1]="0"，通过atoi函数，将字符串"0"，转换为整型0，并传入fcntl函数中，而在unix系统的`<unistd.h>`中有如下宏定义
```C
#define	STDIN_FILENO	0	/* Standard input.  */
#define	STDOUT_FILENO	1	/* Standard output.  */
#define	STDERR_FILENO	2	/* Standard error output.  */
```

即标准输入0,标准输出1,标准错误2，所以，fcntl第一个参数为1(STDIN_FILENO)标准输入时，fcntl将继续接收来自终端的输入，通过输入重定向符号`<`，将`/dev/tty`的文件描述符传入到fcntl函数的第一个参数，由于此时程序只是读入该文件的信息，故返回`O_RDONLY`

第二行命令`./a.out 1 > temp.foo`同理，1是标准输出，由于此时fcntl接收temp.foo文件描述符，由于此时要写入，且覆盖写入，所以返回`O_WRONLY`

第三行命令`cat temp.foo`打开文件查看信息

第四行命令`./a.out 2 2>>temp.foo`由于传入fcntl的参数是2(STDERR_FILENO)，而错误信息使用命令`2>>`追加重定向到temp.foo中，所以fcntl第一个参数将为temp.foo的文件描述符，由于是追加写入，所以返回值为`O_WRONLY|O_APPEND`，标准输出依然为终端，所以将C程序中的`printf`出的信息依然输出到终端

第五行命令`./a.out 5 5<>temp.foo`中子句`5<>temp.foo`表示在文件描述符5上打开文件temp.foo以供读写
