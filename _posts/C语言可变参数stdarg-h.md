---
title: C语言可变参数stdarg.h
date: 2020-04-14 10:54:25
tags:
- 可变参数
categories:
- C语言
description:
- stdarg.h中提供了可变参数的写法
---

<!--more-->

## 整体步骤
* 提供一个省略号的函数原型
* 在函数定义中创建一个`va_list`类型的变量
* 用宏把该变量初始化为一个参数列表
* 用宏访问参数列表
* 用宏完成清理工作

### 函数原型
这种函数的原型应该有一个形参列表，其中至少有一个形参和一个省略号，比如，省略号必须是最后一个参数，省略号的前一个参数在标准中用`parmN`术语来描述
```c
void f1(int n, ...);
void f2(const char *s, int k, ...);
```

### 创建va_list变量
接下来用声明在`stdarg.h`中的类型来创建数据对象
```c
double Sum(int lim, ...)
{
    va_list ap;  //声明一个存储参数的对象
```

在本例中，`lim`是`parmN`形参，它表明变参列表中参数的数量

### 初始化为参数列表
然后，使用`va_start`宏将参数列表拷贝到`va_list`的变量中
该宏有两个参数：`va_list`类型的变量和`parmN`形参
```c
va_start(ap, lim);  //把ap初始化为参数列表
```

### 访问参数列表
使用`va_arg()`宏，该宏接受两个形参：一个`va_list`类型的变量和一个类型名，第一次调用`va_arg()`时，它返回参数列表的第一项，第二次返回第二项，以此类推，表明类型的参数指定了返回值的类型
假设第一个类型是`double`，第二个是`int`，你可以这样做
```c
double tic;
int toc;
...
tic = va_arg(ap, double);
toc = va_arg(ap, int);
```

> 注意，宏参数类型必须和传入参数类型匹配，如果第一个参数是10.0，可以正常运行，而如果参数是10，这行代码可能出错，这里不会像赋值那样把double自动转换为int

### 清理工作
最后用`va_end()`来清理内存，确保堆栈的正确恢复，它接收一个`va_list`参数
```c
va_end(ap);
```

## 示例程序
* 求和程序
```c
#include <stdarg.h>
#include <stdio.h>
double Sum(unsigned n, ...)
{
    va_list va;
    va_start(va, n);
    double sum;
    for (int i = 0; i < n; i++)
        sum += va_arg(va, double);
    return sum;
}
int main(void)
{
    printf("%f", Sum(3, 1.2, 1.3, 1.4));
}
```

结果输出
```
3.900000
```

* 字符串赋值程序
这里介绍另一个函数`vsnprintf()`，其声明如下
```c
int vsnprintf (char *__s, size_t __maxlen, const char *__format, __gnuc_va_list __arg);
```

参数说明
`char *__s：`把生成的格式化的字符串存放在这里.
`size_t __maxlen`：str可接受的最大字符数(非字节数，UNICODE一个字符两个字节),防止产生数组越界.
`const char *__format`：指定输出格式的字符串，它决定了你需要提供的可变参数的类型、个数和顺序。
`__gnuc_va_list __arg`：va_list变量. va:variable-argument:可变参数

返回值
执行成功，返回最终生成字符串的长度，若生成字符串的长度大于size，则将字符串的前size个字符复制到str，同时将原串的长度返回（不包含终止符）；执行失败，返回负值，并置errno.

示例
```c
#include <stdarg.h>
#include <stdio.h>
int String(char* dest, const char* fmt, ...)
{

    va_list ap;
    va_start(ap, fmt);
    int len = vsnprintf(dest, 100, fmt, ap);
    va_end(ap);
    return len;
}
int main(void)
{
    char str[101];
    int a = 23, b = 22;
    String(str, "%d + %d = %d\n", a, b, a + b);
    printf("%s\n", str);
}
```
