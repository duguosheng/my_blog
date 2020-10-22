---
title: C++变参函数
date: 2020-05-06 13:57:16
tags:
- 变参函数
- C++
categories:
- C/C++
---

为了能处理不同数量实参的函数，C++11标准提供了两种主要的方法
* 所有参数类型相同：initializer_list形参
* 参数类型不同：可变参数模板
此外还有省略号形参这个写法，这种函数一般只用于与C函数交互的接口程序
<!--more-->


## initializer_list形参
`initializer_list`是标准库的类型，用于表示某种特定类型的值的数组，它定义在同名的头文件中
提供了如下操作

| 代码                                 | 含义                                                     |
|--------------------------------------|----------------------------------------------------------|
| initializer_list<T> lst;             | 默认初始化，T类型元素的空列表                            |
| initializer_list<T> lst{a, b, c...}; | lst的元素元素依次是对应初始值的副本，列表中的元素是const |
| lst2(lst)和lst2 = lst                | 拷贝或赋值不会拷贝列表中的元素，原始列表和副本共享元素   |
| lst.size()                           | 元素数量                                                 |
| lst.begin()                          | 返回指向lst中首元素的指针                                |
| lst.end()                            | 返回指向lst中尾元素下一位置的指针                        |

> 和`vector`不一样，`initializer_list`中的值始终是常量，无法改变

例如，我们可以编写输出错误信息的函数

```cpp
void error_msg(initializer_list<string> il) {
    for (auto iter = il.begin(); iter != il.end(); ++iter)
        cout << *iter;
    cout << endl;
}
int main(void) {
    const string error1("错误信息1");
    const string error2("错误信息2");
    error_msg({"程序错误：", error1});
    error_msg({"程序错误：", error2});
    return 0;
}

```

## 可变参数模板
一个可变参数模板就是一个接受可变参数的模板函数或模板类。
可变数目的参数被称为**参数包**，存在两种参数包：**模板参数包：**表示0或多个模板参数; **函数参数包：**表示0或多个函数参数

`class...`或`typename...`指出接下来的参数表示0或多个类型的列表
类型名后面跟省略号表示0或多个给定类型的非类型参数的列表

```cpp
// Args是模板参数包，rest是函数参数包
// Args表示0或多个模板类型参数
// rest表示0或多个函数参数
template <typename T, typename... Args>
void foo(const T& t, const Args&... rest);
```

编译器从实参推断模板参数类型，例如

```cpp
int i = 0;
string s("hello");
foo(i, s, " world");
```

`foo()`被推断为

```cpp
foo(const int &, const string&, const char[6]&);
```

### sizeof...运算符
通过此运算符可以获知包中有多少个元素，例如上例中获取`Args`包中的元素数量

```cpp
sizeof...(Args);
```

### 递归的变参函数
可变参数的函数通常是递归的，第一步调用处理包中第一个实参，然后用剩余实参调用自身。

* 我们定义一个`print`函数打印信息
为了终止递归，我们还需要编写一个非可变参数的`print`函数

```cpp
//终止递归并打印最后一个元素
//必须在变参的print前声明
template <typename T>
ostream& print(ostream& os, const T& t) {
    return os << t;
}
//包中除了最后一个元素之外都将调用这个print
template <typename T, typename... Args>
ostream& print(ostream& os, const T& t, const Args&... rest) {
    os << t << ", ";            //打印第一个实参
    return print(os, rest...);  //递归调用，打印其他实参
}
```

假设调用了`print(cout, i, s, 42);`

递归过程如下

| 调用                  | t                     | rest... |
|-----------------------|-----------------------|---------|
| print(cout, i, s, 42) | i                     | s, 42   |
| print(cout, s, 42)    | s                     | 42      |
| print(cout, 42)       | 调用非变参版本的print |         |

最后一个调用中，两个`print`提供同样好的匹配程度，而因为非变参的更加特例化，因而调用它。

### 包扩展
对于一个参数包，除了获取大小，我们唯一能做的就是扩展它。当扩展一个包时，我们还要提供用于每个扩展元素的模式。
扩展一个包就是把它分解为构成的元素，对每个元素应用模式，获得扩展后的列表。我们通过在模式右边放一个省略号`...`来触发扩展操作。


