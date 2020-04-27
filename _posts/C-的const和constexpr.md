---
title: C++的const和constexpr
date: 2020-04-24 20:40:22
tags:
- const
- constexpr
categories:
- C/C++
description:
- 用于定义不可改变的量
---

<!--more-->

## const
* 因为const对象一旦创建后就不能改变，所以必须初始化
* 一般变量的定义不能使用`extern`关键字，而如果想在多个文件之间共享`const`对象，必须在变量的定义之前加`extern`
```cpp
//file1.cc定义
extern const int BUFFER_SIZE = fcn();
//file1.h声明
extern const int BUFFER_SIZE;
```

### 顶层const
顶层const表示指针本身是个常量，底层const表示指针所指的对象是一个常量。
```cpp
int i = 0;
int *const p1 = &i;        //不能改变p1，顶层const
const int ci = 42;         //不能改变ci，顶层const
const int *p2 = &ci;       //可以改变p2，底层const
const int *const p3 = p2;  //左const为底层，右const为顶层
const int &r = ci;         //声明引用的const都是底层const
```

## constexpr和常量表达式
常量表达式是指值不会改变并且在编译过程中就能得到计算结果的表达式。
C++11允许将变量声明为`constexpr`类型以便编译器来验证变量是否是一个常量表达式。声明为`constexpr`的变量一定是一个常量，并且用常量表达式初始化
```cpp
constexpr int mf = 20;         //20是常量表达式
constexpr int limit = mf + 1;  //mf+1是常量表达式
constexpr int sz = size();     //只有size()是constexpr函数时才正确
```

### constexpr函数
是指能用于常量表达式的函数，定义`constexpr`函数的方法与其他函数类似，不过要遵从：函数的返回值及所有形参的类型都得是字面值类型，函数体有且只有一条`return`语句
```cpp
constexpr int new_sz() { return 42; }
constexpr int foo = new_sz();
```

因为编译器能在编译时验证`new_sz`函数返回的是常量表达式，所以可以用它来初始化`foo`
执行初始化任务时，编译器把对`constexpr`函数的调用替换成其结果值，为了在编译过程中随时可以展开，`constexpr`函数隐式被声明为内联的
内联函数和`constexpr`函数通常定义在头文件中

```cpp
constexpr size_t scale(size_t cnt) { return new_sz() * cnt; }
int arr[scale(2)];  //正确
int i = 2;
int arr2[scale(i)]; //错误，scale(i)不是常量表达式
```

### 指针和constexpr
在`constexpr`中如果声明了指针，则其只对指针有效，而对于指针所指的值无效
```cpp
const int *p = nullptr;     //p指向的值为常量
constexpr int *q = nullptr; //q指针为常量
```
