---
title: 'C预处理器'
date: 2020-04-05 09:14:39
tags:
- 预处理
- 变参宏
categories:
- C语言
description:
- #运算符，变参宏，defined运算符，C语言预定义宏，#line和#error
---

<!--more-->
[关于预处理的基础学习](http://www.duguosheng.xyz/2019/11/06/C%E8%AF%AD%E8%A8%80%E7%BC%96%E8%AF%91%E9%A2%84%E5%A4%84%E7%90%86/)

### #运算符
`#`作为一个预处理运算符，可以把记号转换成字符串，例如，如果`x`是一个宏形参，那么`#x`就是转换为字符串`"x"`的形参名
```C
#include <stdio.h>
#define PR1(x) printf("your input is " #x)
int main(void)
{
    PR1(1);
}
```

输出
```
your input is 1
```

字符串串联功能将`"your input is "`和`"1"`两个相邻的字符串合并成了一个字符串

### ##运算符
`##`运算符把两个记号组合成一个记号，例如
```C
#include <stdio.h>
#define XNAME(n) x##n
#define PR(n) printf("x" #n "=%d\n", x##n)
int main(void)
{
    int XNAME(1) = 100;
    int x2 = 200;
    PR(1);
    PR(2);
    return 0;
}
```

宏`XNAME(1)`将展开为`x1`

### 变参宏
C99/C11提供了接收可变参数的工具，通过把宏参数列表中最后的参数写成省略号(即三个点`...`)来实现这一功能，这样预定义宏`__VA_ARGS__`可用在替换部分中，表明省略号代表什么
例如下面的定义
```C
#include <stdio.h>
#define PR_VA(...) printf(__VA_ARGS__)
int main(void)
{
    int x1 = 100;
    int x2 = 200;
    PR_VA("x1 = %d, x2 = %d\n", x1, x2);
    return 0;
}
```
`__VA_ARGS__`将展开为三个参数，`"x1 = %d, x2 = %d\n"`，`x1`，`x2`三个参数
**记住，省略号只能代替最后的宏参数**

### defined预处理符
`#ifdef A`可使用`#if defined(A)`来替代
`#ifndef A`可使用`#if !defined(A)`来替代
使用这种方法的优点是可以和`#elif`一起使用，而前一种不行
```C
#if defined(IBMPC)
    #include "ibmpc.h"
#elif defined(VAX)
    #include "vax.h"
#elif defined(MAC)
    #include "mac.h"
#else
    #include "general.h"
#endif
```

### 预定义宏
C标准规定了一些预定义宏
| 宏                 | 含义                                                   |
|--------------------|--------------------------------------------------------|
| `__DATE__`         | 字符串，预处理日期，mm dd yyyy格式，如Nov 15 2018      |
| `__FILE__`         | 字符串，当前源代码文件名                               |
| `__LINE__`         | 整型，当前行号                                         |
| `__STDC__`         | 当被设置为1时，表名严格遵循C标准                       |
| `__STDC_HOSTED__`  | 本机环境设置为1,否则为0                                |
| `__STDC_VERSION__` | 长整型，支持C99，设置为199901L，支持C11，设置为201112L |
| `__TIME__`         | 字符串，翻译代码的时间，格式hh:mm:ss                   |
| `__func__`         | 字符串，当前函数名                                     |

### #line和#error
`#line`用于重置`__LINE__`和`__FILE__`宏报告的行号和文件名
```C
#line 1000          //把当前行号重置为1000
#line 10 "cool.c"   //把行号重置为10,文件名重置为cool.c
```

`#error`用于发布一条错误信息
```C
//检测C标准
#if __STDC_VERSION__ != 201112L
#error NOT C11
#endif
```



