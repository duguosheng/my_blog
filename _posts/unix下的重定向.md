---
title: unix下的重定向
date: 2020-03-27 23:48:23
tags:
- 重定向
categories:
- linux
---

### 理解
**输出重定向**：一般情况下，我们在终端下执行命令，或者执行程序，如果有输出信息，则会显示在终端上，通过重定向功能，我们可以将这些信息输出到文件中，这是输出重定向的一般用法
**输入重定向**：将文件中的内容作为程序的输入信息

<!--more-->
### 重定向符号
```
>  输出重定向(write)
>> 输出追加重定向(append)
<  输入重定向
<< 输入追加重定向
```

### 标准输入，输出，错误
在unix系统中规定了这三种符号
```
STDIN  0
STDOUT 1
STDERR 2
```

一般来说，这三者都指向终端，即各种信息都由终端输入，输出显示

### 重定向的使用
#### 输出重定向
```bash
echo 123 > test.txt
echo 123 1> test.txt
echo $HOME >> test.txt
echo $HOME 1>> test.txt
```

标准输出`STDOUT`默认是`1`，所以`>`等同于`1>`，`>>`等同于`1>>`

```bash
ls / /123 2>ls.log
```

`2>`，`2>>`中，2表示`STDERR`标准错误，即仅将错误信息重定向到文件
执行上面的命令后，正常信息(`ls /`的输出信息)会在终端显示，而错误信息(`ls /123`没有该目录)会被重定向到`ls.log`文件中
![结果](r1.png)

#### 丢弃输出
有时你需要执行命令，但不想在屏幕上显示输出。在这种情况下，你可以通过重定向到文件`/dev/null`以丢弃输出:

```bash
command > /dev/null
```

在这里 command 是要执行的命令的名字。文件`/dev/null`是一个自动丢弃其所有的输入的特殊文件。

为了丢弃一个命令的输出和它的错误输出，你可以使用标准重定向来将 STDOUT 重定向到 STDERR ：

```bash
command > /dev/null 2>&1
```

`2>&1`表示`2(STDERR)`的重定向位置和`1(STDOUT)`相同

#### 输入重定向
```c
#include <stdio.h>
#include <unistd.h>
#define BUFFSIZE 4096
int main(void) {
    int n;
    char buf[BUFFSIZE];
    while ((n = read(STDIN_FILENO, buf, BUFFSIZE)) > 0)
        if ((write(STDOUT_FILENO, buf, n)) != n) {
            printf("write error.\n");
            return -1;
        }
    if (n < 0) {
        printf("read error.\n");
        return -1;
    }
    return 0;
}
```

`STDIN_FILENO`和`STDOUT_FILENO`分别是定义在`<unistd.h>`下的宏，分别对应于标准输入输出，`read`将从标准输入中依次读取`BUFFSIZE`字节数据，并存入`buf`中，然后`write`将读入的数据再输出到标准输出
而通过重定向可以实现(假设编译的可执行文件为`test`)
* 批量输出(<< EOF)
![示例](r2.png)

图中，前面有`>`的是用户输入的数据，下面的是输出的数据

* 文件复制
```bash
./test < a.txt > b.txt
```

此命令**输入重定向为`a.txt`，输出重定向为`b.txt`**，结果是将`a.txt`的所有内容复制到`b.txt`中

