---
title: C++类的详解
date: 2019-11-09 12:40:59
tags:
- 面向对象
categories:
- C++
description:
- 面向对象既是用类的思想封装属性，用对象的方式封装数据
---

<!--more-->
转载自[码农有道](https://blog.csdn.net/wucz122140729/article/details/98477574)

### 封装文件类
```C++
//文件操作类
class CFile
{
    private:
        FILE *m_fp;  // 文件指针
        bool m_bEnBuffer;  // 是否启用缓冲区，true-启用，false-不启用
        char m_filename[301];  // 文件名

    public:
        //类的构造函数
        CFile();
        //启用，禁用缓冲区
        void EnBuffer(bool bEnBuffer=true);
        //打开文件，成功返回true,失败返回false
        bool Open(const char *filename, const char *openmode);
        //调用fprint向文件写入数据
        void Fprintf(const char *fmt, ...);
        //调用fgets从文件中读取一行
        bool Fgets(char *strBuffer, const int ReadSize);
        //关闭文件指针
        void Close();
        //类的析构函数
        ~CFile();
};
```

类的声明和成员函数的定义都是类定义的一部分，在实际开发中，我们通常将类的声明放在头文件中，而将成员函数的定义放在源文件中。

### 类成员的访问权限
C++通过 public、protected、private 三个关键字来控制成员变量和成员函数的访问权限，它们分别表示公有的、受保护的、私有的，被称为成员访问限定符。所谓访问权限，就是类外面的代码该类中成员权限。

在类的内部（定义类的代码内部），无论成员被声明为 public、protected 还是 private，都是可以互相访问的，没有访问权限的限制。

在类的外部（定义类的代码之外），只能通过对象访问public的成员，不能访问 private、protected 属性的成员。

本节重点介绍 public 和 private，protected 将在以后介绍。

private 后面的成员都是私有的，如m_fp和m_bEnBuffer，直到有 public 出现才会变成共有的；public 之后再无其他限定符，所以 public 后面的成员都是共有的。

private 关键字的作用在于更好地隐藏类的内部实现，该向外暴露的接口（能通过对象访问的成员）都声明为 public，不希望外部知道、或者只在类内部使用的、或者对外部没有影响的成员，都建议声明为 private。

声明为 private 的成员和声明为 public 的成员的次序任意，既可以先出现 private 部分，也可以先出现 public 部分。如果既不写 private 也不写 public，就默认为 private。

在一个类体中，private 和 public 可以分别出现多次。每个部分的有效范围到出现另一个访问限定符或类体结束时（最后一个右花括号）为止。

你可能会说，将成员变量全部设置为 public 省事，确实，这样做 99.9% 的情况下都不是一种错误，我也不认为这样做有什么不妥；但是，将成员变量设置为 private 是一种软件设计规范，尤其是在大中型项目中，还是请大家尽量遵守这一原则。

### 成员变量的命名
成员变量大都以`m_`开头，这是约定成俗的写法，不是语法规定的内容。以`m_`开头既可以一眼看出这是成员变量，又可以和成员函数中的参数名字区分开。

例如成员函数EnBuffer的函数体如下：
```C++
void CFile::EnBuffer(bool bEnBuffer)
{
    m_bEnBuffer = bEnBuffer;
}
```

### 构造函数
在CFile类的声明中，有一个特殊的成员函数CFile()，它就是构造函数（constructor）。
```C++
//类的构造函数
CFile();
```

构造函数的名字和类名相同，没有返回值，不能被显式的调用，而是在创建对象时自动执行。

构造函数具备以下特点：
1）构造函数必须是 public 属性。
2）构造函数没有返回值，因为没有变量来接收返回值，即使有也毫无用处，不管是声明还是定义，函数名前面都不能出现返回值类型，即使是 void 也不允许。

构造函数可以有参数，允许重载。一个类可以有多个重载的构造函数，创建对象时根据传递的参数来判断调用哪一个构造函数。但是，在实际开发中，我很少重载构造函数，一是程序写出来不漂亮，二是构造函数不能有返回值，能做的事情很有限，让其它的成员函数去做就是了。

构造函数在实际开发中会大量使用，它往往用来做一些初始化工作，对成员变量进行初始化等，我基本上就是这么用的。
```C++
CFile::CFile()
{
    m_fp = 0;
    m_bEnBuffer = true;
    memset(m_filename, 0, sizeof(m_filename));
}
```

### 析构函数
在CFile类的声明中，还有一个特殊的成员函数~CFile()，它就是析构函数（destructor）。
```C++
//类的析构函数
~CFile();
```

析构函数的名字在类的名字前加~，没有返回值，但可以被显式的调用，在对象销毁时自动执行，用于进行清理工作，例如释放分配的内存、关闭打开的文件等，这个用途非常重要，可以防止程序员犯错。

析构函数具备以下特点：
1）构造函数必须是 public 属性的。
2）构造函数没有返回值，因为没有变量来接收返回值，即使有也毫无用处，不管是声明还是定义，函数名前面都不能出现返回值类型，即使是 void 也不允许。
析构函数不允许重载的。一个类只能有一个析构函数。
```C++
CFile::~CFile()
{
    if(m_fp!=0)
    {
        fclose(m_fp);  // 关闭文件指针
    }
    m_fp = 0;
}
```

### 可变参数
我们已经介绍过printf、fprintf、sprintf、snprintf函数，它们是一组功能相似的函数，并且有一个共同点，就是函数的参数列表是可变的。

函数的声明如下：
```C
int printf(const char *format, ...);
int fprintf(FILE *stream, const char *format, ...);
int sprintf(char *str, const char *format, ...);
int snprintf(char *str, size_t size, const char *format, ...);
```

在实际开发中，我们的自定义函数也需要可变参数，实现类似上述函数的功能，例如CFile类的Fprintf成员函数，函数的声明和定义如下：
```C++
void CFile::Fprintf(const char *fmt, ...)
{
    if(m_fp==0)
        return;

    va_list vlist;              //定义va_list指针
    va_start(vlist, fmt);       //根据fmt的格式，分析参数列表
    vfprintf(m_fp, fmt, vlist); //将分析结果输出到文件
    va_end(vlist);              //释放va_liat指针

    if(m_bEnBuffer==false)
    {
        fflush(m_fp);
    }
}
```

va_list指针、va_start宏、va_end宏难以理解，大家会抄就行，我不详细介绍，vfprintf函数把宏分析的结果输出到文件，还有一系列功能相似的函数，声明如下：

```C++
// 输出的屏幕
int vprintf(const char *format, va_list ap);
// 输出到文件
int vfprintf(FILE *stream, const char *format, va_list ap);
// 输出到字符串
int vsprintf(char *str, const char *format, va_list ap);
// 输出到字符串，第二个参数指定了输出结果的长度，类似snprintf函数。
int vsnprintf(char *str, size_t size, const char *format, va_list ap);
```

可变参数的知识不只这些，但是，在实际开发中，我只见过用于格式化输出，其它的应用场景我没有见过。
