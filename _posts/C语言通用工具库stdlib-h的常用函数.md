---
title: C语言通用工具库stdlib.h的常用函数
date: 2020-04-02 09:09:15
tags:
- stdlib
categories:
- C语言
description:
- 介绍rand(), srand(), malloc(), free(), exit(), atexit(), qsort()的使用
---

<!--more-->
### 生成随机数
#### 函数原型
```C
int rand (void);
void srand (unsigned int __seed);
```

#### 说明
需要用到`rand()`, `srand()`和`<time.h>`中的`time()`函数
* rand函数是"伪随机数生成器"，意思是可预测生成数字的实际序列，但是在生成范围内均匀分布，生成的随机数范围是`0-RAND_MAX`，`RAND_MAX`是`stdlib.h`中的宏
* srand函数:用于重置随机数种子，使得`rand()`生成的随机数各不相同
* time函数：将系统时间作为`srand`的参数

#### 示例
```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int main(void) {
    int rand_arr[20] = { 0 };
    for (int i = 0; i < 20; i++) {
        // 用于for循环执行的很快，所以使用time和rand的和作为参数
        srand((unsigned)time(NULL) + (unsigned)rand());
        // 通过运算符可以限制随机数范围，这里是0-100
        d_arr[i] = rand() % 100;
        printf("%d ", d_arr[i]);
    }
    return 0;
}
```

### 内存管理
参看[另一篇博客](http://www.duguosheng.xyz/2019/10/26/C%E8%AF%AD%E8%A8%80%E5%8A%A8%E6%80%81%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86/)


### exit和atexit
#### 函数原型
```C
void exit (int __status);
int atexit (void (*__func) (void));
```

#### 说明
* exit
用于终止程序，如果在没有递归的`main`函数中使用`exit`等同于使用`return`，不同之处在于即使在其他程序中或递归函数中使用`exit`也会终止程序，而`return`会将控制权交给上一级函数，
`exit`需要一个`int`类型的参数，指明终止状态，在unit系统中，0代表执行成功，其他值代表失败，然而可能并不适用其他系统，所以用两个宏供使用`EXIT_SUCCESS`代表成功，`EXIT_FAILURE`代表失败
当main函数终止时，会隐式调用`exit`

* atexit
指定当调用exit时执行注册的特定函数，`atexit`的参数是一个函数指针，该函数必须无参数无返回值，通过多次调用`atexit`可以注册多个函数，ANSI保证，至少可以放32个函数，而这些函数在调用`exit`时，执行顺序与注册顺序相反，即最后添加的函数最先执行，这些函数一般是完成清理任务，例如更新程序的文件或重置环境变量

#### 示例
```C
#include <stdio.h>
#include <stdlib.h>
void Done1(void);
void Done2(void);
int main(void)
{
    atexit(Done1);
    atexit(Done2);
    printf("main start");
    exit(EXIT_SUCCESS);
}
void Done1(void)
{
    printf("\nexit-->Done1");
}
void Done2(void)
{
    printf("\nexit-->Done2");
}
```

输出
```
main start
exit-->Done2
exit-->Done1
```

可以看到，注册顺序为`Done1->Done2`，执行顺序为`Done2->Done1`


### 排序qsort
#### 函数原型
```C
void qsort(void *base, size_t nitems, size_t size, int (*compar)(const void *, const void*));
```

#### 说明
qsort函数C语言编译器函数库自带的快速排序函数
* 参数
base-- 指向要排序的数组的第一个元素的指针。
nitems-- 由 base 指向的数组中元素的个数。
size-- 数组中每个元素的大小，以字节为单位。
compar-- 用来比较两个元素的函数，即函数指针（回调函数）

* compar参数
compar参数指向一个比较两个元素的函数。比较函数的原型应该像下面这样。注意两个形参必须是`const void *`型，同时在调用compar 函数（compar实质为函数指针，这里称它所指向的函数也为compar）时，传入的实参也必须转换成`const void *`型。在compar函数内部会将`const void *`型转换成实际类型。
```C
int compar(const void *p1, const void *p2);
```
如果compar返回值小于0（< 0），那么p1所指向元素会被排在p2所指向元素的左面；
如果compar返回值等于0（= 0），那么p1所指向元素与p2所指向元素的顺序不确定；
如果compar返回值大于0（> 0），那么p1所指向元素会被排在p2所指向元素的右面。

#### 示例
```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int mycomp(const void* p1, const void* p2);
int main()
{
    double d_rand = 0.0;
    double d_arr[20] = { 0 };
    for (int i = 0; i < 20; i++) {
        srand((unsigned)time(NULL) + (unsigned)rand());
        d_arr[i] = rand() % 10000 / 1000.0;
        printf("%f ", d_arr[i]);
    }
    putc('\n', stdout);
    qsort(d_arr, 20, sizeof(double), mycomp);
    for (int i = 0; i < 20; i++) {
        printf("%f ", d_arr[i]);
    }
}
//从小到大排序，如果更改return的值可以实现从大到小排序
int mycomp(const void* p1, const void* p2)
{
    const double* d1 = (const double*)p1;
    const double* d2 = (const double*)p2;
    if (*d1 > *d2)
        return 1;
    else if (*d1 < *d2)
        return -1;
    else
        return 0;
}
```

可能会疑惑为什么不使用一个标志位参数来确定正向或反向排序，原因是利用函数指针可以实现其他更复杂的排序，如下面的结构体参数
```C
struct Example {
    int a;
    float b;
};
```

你可以通过编写compar函数实现先用a排序，如果a相等，使用b排序
