---
title: C++类型转换
date: 2020-05-03 15:22:57
tags:
- 类型转换
categories:
- C/C++
description:
- 在C++语言中，某些类型之间有关联。如果当程序需要其中一种类型的运算对象时，可以用另一种关联类型的对象或值来替代。
---

<!--more-->

## 隐式转换

```cpp
int ival = 3.145 + 3;  //编译器可能警告损失精度
```

在上面的例子中，3.145是`double`类型，3是`int`类型，二者不能直接相加，3先转换成浮点型，然后进行浮点数相加，所得结果类型是`double`。在初始化的过程中，由于被初始化的对象类型无法改变，结果的`double`类型再转换为`int`，并将小数部分丢弃。
上述的类型转换是自动执行的，被称作**隐式转换**

### 算数转换
算数转换的含义是把一种算数类型转换成另一种算数类型，运算符的运算对象将转换为最宽的类型，例如任何类型和`long double`运算，都会转换为`long double`;当表达式中既有浮点数又有整型时，整数将转换为相应的浮点类型。

* 整型提升
对于`bool, char, short`等类型，会被提升为`int`或`unsigned int`
较大的`char`类型(`wchar_t，char16_t，char32_t`)提升成`int, unsigned int, long, unsigned long, long long , unsigned long long`中最小的一种。

* 无符号类型运算
有符号数与无符号数运算(四则运算、< > == !=等)有符号数将转换为无符号数。这一特点会带来一些意想不到的副作用。

```c
//假设共4位
-7-1 < 7u
```

-7-1的二进制表示为0b1000，当他转换为无符号数时表示8, 8>7, 所以上例中的结果是flase，显然不符合预期

* 数组转换成指针

```cpp
int ia[10];   //含有10个整数的数组
int *ip = ia; //ia转换成指向数组首元素的指针
```

当数组用作`decltype`关键字的参数，或者作为取地址符&，`sizeof`及`typeid`等运算符的运算对象时，上述转换不会发生。


## 显式转换
### 命名强制类型转换
一个命名的强制类型转换具有如下形式

```cpp
cast_name<type>(expression);
```

type：转换的目标类型，如果是引用类型，则结果是左值
expression：要转换的值
cast_name：是`static_cast, dynamic_cast, const_cast, reinterpret_cast`中的一种

#### static_cast
只要不包含底层const，一般都可以使用`static_cast`，例如将一个强转为`double`就可以使表达式执行浮点数除法运算

```cpp
double slope = static_cast<double>(j) / i;
```

* 当将一个较大的算数类型转换为较小的类型时，`static_cast`很有用
一般来说，这样转换编译器回给出警告损失精度，但使用了`static_cast`，他就告诉编译器，我们知道并且不在乎损失的精度，所以编译器不会警告

* 对于编译器无法自动执行的类型转换
例如，我们可以找回存在于`void *`中的值

```cpp
void *p = &d;
double *dp = static_cast<double*>(p);
```

#### const_cast
`const_cast`只能改变对象的底层const

```cpp
const char *pc;
char *p = const_cast<char*>(pc); // 正确，但是通过p写值是未定义的
```

如果去掉了某个对象的const性质，编译器就不再阻止我们的写操作了
如果对象本身不是一个常量，这样是合法的行为
如果对象是常量，通过这样获得写权限进行写入是未定义的行为

> 只有`const_cast`能改变表达式的常量属性，但它不能改变表达式的类型

```cpp
const char* cp;
//错误：static_cast不能转换掉const性质
char* q = static_cast<char*>(cp);
//正确，字符串字面值转换为string
string s1 = static_cast<string>(cp);
//错误，const_cast只能改变常量属性
string s2 = const_cast<string>(cp);
```

* 用法：static_cast常用于有函数重载的上下文中
如我们写了一个比较字符串长短的函数，返回较短的字符串的引用

```cpp
const string& shorterString(const string& s1, const string& s2) {
    return s1.size() <= s2.size() ? s1 : s2;
}
```

该函数接收两个常量引用，返回的也是常量引用。我们当然也可以对两个非常量的string实参调用这个函数，但返回的仍然是`const string`的引用。因此我们需要一个新的重载函数，接收非常量，返回的也是非常量
```cpp
string& shorterString(string& s1, string& s2) {
    auto& r = shorterString(const_cast<const string&>(s1),
                            const_cast<const string&>(s2));
    return const_cast<string&>(r);
}
```

#### reinterpret_cast
`reinterpret_cast`通常为运算对象的位模式提供较低层次上的重新解释

```cpp
int *ip;
char *pc = reinterpret_cast<char*>(ip);
string str(pc);  //可能会导致错误
```

使用`reinterpret_cast`是非常危险的，可能会导致运行时错误


### 旧式类型转换
根据所涉及的类型不同，旧式的类型转换分别与各个命名强制类型转换有相似的行为，但旧式的类型转换从表现形式上来说不那么清晰明了，所以建议使用命名式。
