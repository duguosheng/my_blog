---
title: C++引用
date: 2019-11-09 14:22:47
tags:
- 引用
categories:
- C/C++
description:
-  引用就是某一变量（目标）的一个别名
---

<!--more-->

转自[码农有道](https://blog.csdn.net/wucz122140729/article/details/98477620)
  对于习惯使用C进行开发的程序员来说，在看到C++中出现的&符号，可能会犯迷糊，因为在C语言中这个符号表示了取地址符，但是在C++中，它除了取地址，还有其它的用途，叫做引用（reference），引用是C++引入的新语言特性。

### 引用的概念
引用就是某一变量（目标）的一个别名，对引用的操作与对变量直接操作完全一样。
* 引用的声明方法：类型标识符 &引用名=目标变量名；
例如：
```C++
int a;
int &ra=a;  // 定义引用ra，它是变量a的引用，即别名。
```
说明：
1）&在此不是求地址运算，而是起标识作用。
2）类型标识符是指目标变量的类型。
3）声明引用时，必须同时对其进行初始化，否则编译器会报错。
4）引用声明后，相当于目标变量名有两个名称，即该目标原名称和引用名，且不能再把该引用名作为其他变量名的别名。
```C++
ra=1;  //等价于  a=1;
```
5）声明一个引用，不是新定义了一个新的变量，它只表示该引用名是目标变量名的一个别名，它本身不是一种数据类型，因此引用本身不占存储单元，系统也不给引用分配存储单元。

* 引用可以用const修饰，表示只读，用这种方式声明的引用，不能通过引用对目标变量的值进行修改。
```C++
int a;
const int &ra=a;
a=10;    // 可以
ra=10;   // 不行
```

### 引用的应用
引用的主要作用就是作为函数的参数。以前的C语言中函数参数传递是值传递，如果有大块数据作为参数传递的时候，采用的方案是数据的地址。但是在C++中，又增加了一种同样有效率的选择，就是引用。

* 示例
```C++
#include <stdio.h>

void print_num(int &num);
int main()
{
    int number = 10;
    print_num(number);
    printf("%d\n", number);
}

void print_num(int &num)
{
    num = 20;
    printf("%d\n", num);
}
```

> 运行结果
```
20
20
```

> 其执行过程相当于
```C++
int number = 10;
{
    int &num = number;
    num = 20;  // 改变引用的值
    printf("%d\n", num);
}
printf("%d\n", number);
```

从以上的示例可以看出，**传递引用给函数与传递指针的效果是一样的**。这时，被调函数的参数就成为调用者调函数中的变量或对象的一个别名来使用，所以在被调函数中对引用的操作就是对目标变量的操作。

* 引用的优点：
引用传递（pass by refenrence），在内存中没有产生形参。效率提高！也不用处理指针的析构问题。

在很多资料中，把引用的优点过于夸大，在我看来，引用的好处就是调用函数的时候，不用填写取地址符&，子函数中也不写取变量符`*`，结构体和类不用->取成员。我更倾向传递地址的方式，因为更直观，不管是在函数内部还是函数被调用的地方，一眼就能清楚是否是地址。

引用还可以作为函数的返回值，但我不建议这么用，我实在看不出这样做有什么好处，那就没必要把事情搞得那么复杂，所以这里就不介绍了。
