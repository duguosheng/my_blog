---
title: C语言泛型选择，内联函数，_Noreturn函数
date: 2020-04-08 09:57:40
tags:
- 泛型
- 内联函数
- _Noreturn函数
categories:
- C语言
---

### 泛型选择
#### 关于泛型
**泛型编程**指那些没有指定特定类型，但是一旦指定一种类型，就可以转化成指定类型的代码
C11新增了泛型选择表达式，可根据表达式的类型选择一个值
泛型表达式不预处理指令，但在一些泛型编程中他常用作`#define`宏定义的一部分

<!--more-->
#### _Generic关键字
`_Generic`是C11新增的关键字，其后面的圆括号中包含多个用逗号分隔的项。第一个项是一个表达式，后面的每个项都由一个类型，一个冒号和一个值组成，如`float: 1`，如果没有与类型匹配的标签，表达式的值就是`default:`标签后面的值
宏定义必须为一条逻辑行，但可以使用`\`分成多条物理行

* 示例
```C
#include <stdio.h>
#include <math.h>
#define MYTYPE(x) _Generic((x),        \
                           int         \
                           : "int",    \
                           float       \
                           : "float",  \
                           double      \
                           : "double", \
                           default     \
                           : "other")
//使用泛型选择不同的数学函数
#define SQRT(x) _Generic((x),        \
                         float       \
                         : sqrtf,    \
                         long double \
                         : sqrtl,    \
                         default     \
                         : sqrt)(x)
int main(void)
{
    int d = 5;
    printf("%s\n", MYTYPE(d));
    printf("%s\n", MYTYPE(2.0 * d));
    printf("%s\n", MYTYPE(3L));
    printf("%s\n", MYTYPE(&d));
}
```

> 如果使用gcc编译使用到了数学库，需要后面加上`-lm`参数

* 对于`SQRT`，先由`_Generic`获得一个函数指针，再通过指针调用对应的函数

---

### 内联函数
通常，函数调用都有一定的开销，包括建立函数、传递参数、跳转到函数代码并返回。使用宏使代码内联，可以避免这样的开销，C99还提供了另一种方法：内联函数
标准规定：具有内部连接的函数可以成为内联函数，内联函数和调用必须在同一个文件，因此最简单的方法是使用函数说明符`inline`和存储类别说明符`static`(内部链接)
如果有多个文件都要使用一个内联函数，最简单的方法是把内联函数定义(不能只是声明)放入头文件
* 示例
```C
#include <stdio.h>
inline static int Add(int x, int y)
{
    return x + y;
}
int main(void)
{
    int a = 100, b = 25;
    printf("%d", Add(a, b));
}

```

* 注意事项
由于并未给内联函数预留代码块，所以无法获得其地址(实际上可以获得，但是这样编译器会生成一个非内联函数)
内联函数应该比较短小
一般不在头文件放置可执行代码，内联函数是个例外

---

### _Noreturn函数
`_Noreturn`和`inline`都是函数说明符，这个关键字表明不会将控制权返回给主调函数，告诉用户以免滥用该函数
* 示例
```C
#include <stdio.h>
#include <stdlib.h>
_Noreturn void Print()
{
    printf("由于使用了_Noreturn关键字\n");
    printf("这个函数将不会将控制权返回上一级\n");
    printf("需要调用exit来实现退出\n");
    exit(EXIT_SUCCESS);
}
int main(void)
{
    printf("----- main start -----\n");
    Print();
    printf("----- main end -----\n");
}
```

* 程序输出
```
----- main start -----
由于使用了_Noreturn关键字
这个函数将不会将控制权返回上一级
需要调用exit来实现退出
```
