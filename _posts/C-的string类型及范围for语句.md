---
title: C++的string类型及范围for语句
date: 2020-04-27 21:35:19
tags:
- string
- 范围for语句
categories:
- C/C++
description:
- 标准库string类型表示可变长的字符序列
---

<!--more-->

## 命名空间的using声明
`string`类型包含在命名空间`std`中，如果要使用，应当指明`std::string`，或者在前面使用`using`声明
```cpp
using std::string;
```

之后，就可以直接使用`string`类型而不必说明命名空间

* 但应当注意：在头文件中不应包含`using`声明
头文件往往被其他文件包含进去，如果使用了`using`声明，可能导致意想不到的命名冲突，所以`.h`文件中每个都要写成`std::string`

## string类型

### 初始化
```cpp
string s1;           //默认初始化，s1是一个空串
string s2(s1);       // s2是s1的副本
string s2 = s1;      //等价于s2(s1)
string s3("value");  // s3是字面值"value"的副本
string s3 = "value"; //等价于s3("value")
string s4(5, 'c');   //把s4初始化为连续5个c组成的串
```


如果使用`=`初始化一个变量，实际执行的是拷贝初始化，编译器将右侧的初始值拷贝到新创建的对象当中去
如果不用`=`，则是直接初始化
```cpp
string s5 = "hello";    //拷贝初始化
string s6("hello");     //直接初始化
```

### 操作

| 操作           | 说明                                          |
|----------------|-----------------------------------------------|
| os<<s          | 将s写到输出流os中，返回os                     |
| is>>s          | 从is中读取一行赋给s，字符串以空白分隔，返回is |
| getline(is, s) | 从is中读取一行赋给s，返回is                   |
| s.empty()      | 是否为空                                      |
| s.size()       | 字符个数                                      |

### 字面职常量和string相加
当把`string`对象和字符字面值及字符串字面值混在一条语句中使用时，必须确保每个加法运算符两侧的运算对象至少有一个是`string`
```cpp
string s1 = "hello";
string s2 = s1 + ", ";
string s3 = s2 + "world" + "!"; //正确，s2+"world"返回一个string，然后继续相加
string s4 = "hello" + ", " + s1; //错误，两个字面量不能相加
```

string是一个类，它定义了加法操作，而字符串字面值与string是不同的类型

### 下标操作
可以使用下标操作对应的字符，计数从0开始


## 范围for语句
C++11新标准提供了范围for语句以供遍历元素
```cpp
string str("somethine");
for(auto c : str)
    cout << c <<endl;
```

如果想要改变字符串中的字符，可以配合引用
```cpp
string str("hello");
for(auto &c : str)
    c = toupper(c);
cout << str << endl;
```


