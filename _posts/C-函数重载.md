---
title: C++函数重载
date: 2019-11-08 23:45:04
tags:
- 重载
categories:
- C++
description:
- 即同名而功能类似的函数
---

<!--more-->
### 重载概念
重载函数是函数的一种特殊情况，为方便使用，C++允许在同一范围中声明几个功能类似的同名函数，但是这些同名函数的形式参数（指参数的个数、类型或者顺序）必须不同，也就是说用同一个函数完成不同的功能。这就是重载函数。重载函数常用来实现功能类似而所处理的数据类型不同的问题。不能只有函数返回值类型不同。

### C++实现函数重载原理
C++代码在编译时会根据参数列表对函数进行重命名，例如：
```C
bool writetofile(char *filename,int value); 
```

重命名为
```C
bool _writetofile_char_int(char *filename,int value); 
```

```C
bool writetofile(char *filename,long value);  
```

重命名为
```C
bool _writetofile_char_long(char *filename,long value);  
```

函数被调用时，编译器会根据传入的实参去逐个匹配，以选择对应的函数，如果匹配失败，编译器就会报错，这叫做重载决议（Overload Resolution）。

> 不同的编译器有不同的重命名方式，这里仅仅举例说明，实际情况可能并非如此。

从这个角度讲，函数重载仅仅是语法层面的，本质上它们还是不同的函数，占用不同的内存，入口地址也不一样。
