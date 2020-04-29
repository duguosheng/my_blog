---
title: C++遍历数组
date: 2020-04-28 10:40:07
tags:
- 数组
- C++
categories:
- C/C++
description:
- 介绍遍历多维数组
---

<!--more-->

首先，我们定义一个二维数组
```cpp
int ia[3][4] = {{1}, {1, 2}, {1, 2, 3}};
```

## 使用别名
读，写和理解一个指向多维数组的指针是一项繁琐的工作，使用别名可以简化一些
```cpp
//以下是两条等价的声明
using int_array = int[4];
typedef int int_array[4];
```

接下来就使用`int_array`进行遍历
```cpp
//输出每个元素的值
for (int_array *p = ia; p != ia + 3; ++p) {
    for (int *q = *p; q != *p + 4; ++q)
        cout << *q << " ";
    cout << endl;
}
```

## 使用auto
C++11新标准指出，通过使用`auto`或`decltype`就能尽可能避免在数组前面加上一个指针类型了
```cpp
for (auto p = ia; p != ia + 3; ++p) {
    for (auto q = *p; q != *p + 4; ++q)
        cout << *q << " ";
    cout << endl;
}
```

## 使用迭代器
使用标准库`<iterator>`中的函数`begin()`和`end()`也可以实现相同的功能，且更简洁一些
```cpp
for (auto p = begin(ia); p != end(ia); ++p) {
    for (auto q = begin(*p); q != end(*p); ++q)
        cout << *q << " ";
    cout << endl;
}
```
