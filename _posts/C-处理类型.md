---
title: C++处理类型
date: 2020-04-27 13:43:54
tags:
- 类型别名
- auto关键字
- decltype关键字
categories:
- C/C++
description:
- 随着程序越来越复杂，程序用到的类型也越来越复杂
---

<!--more-->

## 类型别名
### typedef
```cpp
typedef double wages;  //wages是double的同义词
typedef wages base, *p;  //base是double的同义词，p是double*的同义词
```

含有`typedef`的语句定义的不是变量而是类型别名

### using
新标准规定了一种新的方法，使用别名声明
```cpp
using size = std::string::size_type;
```

这种方法使用`using`作为别名声明的开始，其后紧跟别名和等号，其作用是把左侧的名字规定成等号右侧类型的别名。

### 指针，常量和类型别名
```cpp
typedef char *pstring;
const char *cstr1 = 0;
const pstring cstr2 = 0;
```

再上例中，人们可能会误以为cstr1和cstr2是等价的，实际上，这两种定义截然不同，cstr1的`const`是底层的，即值为常量，而cstr2为顶层`const`，即指针为常量，后者相当于
```cpp
char *const cstr2 = 0;
```

## auto类型说明符
C++11新标准引入了`auto`类型说明符，用它就能让编译器替我们分析表达式所属的类型，`auto`让编译器通过初始值来推算变量类型，因此`auto`定义的变量必须有初始值

```cpp
auto num = func();  //假设func()返回float，则num类型就是float
```


使用`auto`也能在一条语句中声明多个变量，该语句中所有变量的初始基本数据类型必须一致
```cpp
auto i = 0, *p = &i;     //正确，i是整数，p是整数指针
auto sz = 0, pi = 3.14;  //错误，类型不一致
```

### 复合类型，常量和auto
编译器推断出来的`auto`类型有时候和初始值的类型不完全一样，编译器会适当的改变结果使其更符合初始化规则

* 引用
当引用备用作初始值时，真正参与初始化的其实是引用对象的值，此时编译器以引用对象的类型作为`auto`的类型
```cpp
int i = 0, &r = i;
auto a = r;  //a为int类型
```

对于a的定义相当于
```cpp
int a = r;
```

而不是
```cpp
int &a = r;
```

* const
`auto`一般忽略顶层const，保留底层const

```cpp
const int ci = i, &cr = ci;
auto b = ci;  //b是int类型(忽略了顶层const)
auto c = cr;  //c是int类型(cr是ci引用，ci本身是顶层const)
auto d = &i;  //d是一个int*类型
auto e = &ci; //e是指向const int的指针(对常量取地址是一种底层const)
```

如果希望`auto`是顶层const，可以明确指出
```cpp
const auto f = ci;
```

还可以将引用的类型设为`auto`
```cpp
auto &g = ci;
auto &h = 42;        //错误，不能将非常量引用绑定到字面值
const auto &j = 42;  //正确
```

## decltype类型指示符
C++11新标准引入，它的作用是选择并返回操作数的数据类型，在此过程中，编译器分析表达式并得到其类型，却不实际计算表达式的值
```cpp
decltype(f()) sum = x;  //sum类型就是函数f的返回类型
```

注意，这里并不实际调用f()，而只是识别其返回类型

* 对比auto
`decltype`处理类型和`auto`有些不同，如果`decltype`使用的表达式是一个变量，则返回**包括顶层const和引用在内的**该变量的类型。
```cpp
const int ci = 0, &cj = ci;
decltype(ci) x = 0;   //x类型是const int
decltype(cj) y = x;   //y类型是const int&，绑定到x
decltype(cj) z;       //错误，z是一个引用，必须初始化
```

* decltype和引用
如果`decltype`使用的表达式不是一个变量而是表达式，则返回表达式结果对应的类型
```cpp
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;   //正确，加法的结果是int，因此b类型为int
decltype(*p) c;      //错误，c是一个引用，必须初始化
```

如`decltype(*p)`所示的那样，如果表达式的内容是解引用操作，则`decltype`将得到引用类型，正如我们熟悉的那样，`*p`解引用可以得到对象的值，并可以给对象赋值，所以`decltype`得到的是`int&`而不是`int`

`decltype`和表达式形式密切相关，如果给变量名加上了一对或多对括号，编译器就把它当成了表达式，这样的`decltype`会得到引用类型
```cpp
decltype((i)) d; //错误，d是int&，必须初始化
decltype(i) e;   //正确，e是int
```

> 切记：`decltype((variable))`结果永远是引用，而`decltype(variable)`只有当`variable`本身是引用时才是引用
