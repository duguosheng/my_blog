---
title: C语言main函数的参数
date: 2019-10-25 13:44:23
tags:
- main函数
categories:
- C语言
description:
- main函数中包含三个参数
---

<!--more-->

修改自[码农有道的博客](https://blog.csdn.net/wucz122140729/article/details/98435291)

## main函数的参数
 main函数有三个参数，argc、argv和envp表示。
```C
int argc，用于存放命令行参数的个数。
char *argv[]，是个字符串的数组，每个元素都是一个字符指针，指向一个字符串，即命令行中的每一个参数。
char *envp[]，也是一个字符串的数组，这个数组的每一个元素是指向一个环境变量的字符指针。
```

### argc和argv

* 示例
```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    printf("argc is %d\n", argc);
    for(int i=0; i<argc; i++)
    {
        printf("argv[%d] is %s\n", i, argv[i]);
    }
}
```

> 终端下执行并输出
![结果](m1.png)

1）argc的值是参数个数加1，因为程序名称是程序的第一个参数，即argv[0]，在上面的示例中，argv[0]是./main1
2）main函数的参数，不管是书写的整数还是浮点数，全部被认为是字符串。
3）参数的命名argc和argv是程序员的约定，你也可以用argd或args，但是不建议这么做。

### 用于提示信息
```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[])
{
    if(argc!=4)
    {
        printf("输入参数个数不匹配\n");
        printf("这是一个选秀的程序，根据输入的信息为其评定\n");
        printf("用法： ./main2 age appearance character\n");
        printf("例如： ./main2 18 漂亮 开朗\n");
        printf("age          年龄\n");
        printf("appearance   颜值\n");
        printf("character    性格\n");

        return -1;
    }

    if((atoi(argv[1])>=18 && atoi(argv[1])=<25)
            && strcmp(argv[2], "漂亮")==0
            && strcmp(argv[3], "开朗")==0)
    {
        printf("选秀合格");
    }
    else
    {
        printf("还需努力");
    }

    return 0;
}

```

> 当输入参数个数不满足预期设定时
![弹出提示信息](m2.png)

> 当输入满足预期时
![正确执行](m3.png)


### envp
> 存放当前程序运行环境的参数 
```C
#include <stdio.h>


int main(int argc, char *argv[], char *envp[])
{
    int num = 0;
    while(envp[num++]!=0) //数组最后一个元素是0
        printf("%s\n", envp[num]);
}
```

> 运行结果(部分截图)
![环境变量](m4.png)
