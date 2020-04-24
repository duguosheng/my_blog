---
title: C++的string类
date: 2019-11-09 18:59:41
tags:
- string类
categories:
- C/C++
description:
- 字符串
---

<!--more-->
在C语言中，用0结尾的字符数组表示字符串，C的字符串有一个问题，就是定义后大小不能改变，存入的内容只能比数组小，不能大，如果不小心存多了，会引起内存的溢出，这个问题让程序员很郁闷。

C++的string部分的解决了这个问题，它会随存放字符的长度自动伸缩，程序员不必担心内存溢出的问题。string类还和c语言的字符串之间可以转换。

### string的声明
首先，为了在程序中使用string类，必须包含头文件 <string>。如下：
```C++
#include <string>
```

注意这里不是string.h，string.h是C字符串头文件。
string类是一个模板类，位于std命名空间内，为方便使用还需要在程序中增加：
```C++
using namespace std;
```

创建一个string字符串对象很简单：
```C++
string str;
```

如果不指定命名空间，也就是说没有using namespace std，创建字符串对象的方法如下：
```C++
std::string str;
```

### string的赋值
* 示例
```C++
#include <stdio.h>
#include <string>
using namespace std;

int main()
{
    string str;
    str = "小明";  //直接用=赋值
    printf("%s\n", str.c_str());  //下面讲c_str()
}
```

### string的重载的操作符
> 可以用=直接赋值。
> 可以用 ==、>、<、>=、<=、和!=比较字符串。
> 可以用+或者+=操作符连接两个字符串。
> 可以用[]获取特定的字符，类似数组。

最重要的一个成员函数
```C++
const char *c_str(); 
```

返回一个以NULL结尾的c字符串，即c_str()函数返回一个指向正规C字符串的指针, 内容与本string串相同，用于string转`const char*`
```C++
std::string str1;
str1="hello world";
char str2[31];
strcpy(str2,str1.c_str());
```

### string特性描述函数
可用下列函数来获得string的一些特性：
```C++
int size();             // 返回当前字符串的大小
int length();           // 返回当前字符串的长度,结果和上面的size()一样
void clear();           // 清空字符串
```

* string的其它成员函数
string提供了上百个成员函数，非常丰富，丰富得有点眼花，各位自己找资料，有空的时候再研究一下，断章取义的使用，注意，是有空的时候。

### string的本质
string是一个类，通过动态分配内存，实现对字符串的存储，我们来看以下代码。

* 示例
```C++
#include <stdio.h>
#include <string>
using namespace std;

int main()
{
    string str;
    str = "aaaaaaaaaaaaaaa";
    printf("%p\n", str.c_str());
    str += "bbbb";  //字符串增大，需要新的内存空间
    printf("%p\n", str.c_str());
    str = "cccc";  //字符串减小，无须开辟新内存空间
    printf("%p\n", str.c_str());
    str = "dhasjhkashdjsadsadasdashdkhsaddjas";  //字符串增大，需要开辟新内存空间
    printf("%p\n", str.c_str());
}
```
       

> 运行结果
```
0x7ffc20fc1e80
0x5586c80742c0
0x5586c80742c0
0x5586c80742f0
```

通过以上的例子，我们可以看到，string对象用于存放字符的内存地址是变化的，不是什么神奇的技术。

### 应用经验
C++的string类是一个变长的字符串，不需要程序员担心内存溢出的问题，还提供了很多字符串操作函数，初学者可能会想，用它取代C语言中的字符串（以0结尾的字符数组）一定是个很好的主意。我要告诉各位，这是不可能的，因为string中的字符串存储的内存空间没有固定的位置（它也没办法有固定位置）。

对初学者来说，只会用C和C++写一些简单的程序，做一些简单的事情，不懂得实际开发的需求，例如Oracle和MySQL数据库提供的接口，在数据交换的时候需要绑定一个固定的地址，string是做不到的。

我的建议是采用string存放一些需要动态分配内存的临时数据，避开动态内存技术带来的坑，然后转换为C的字符串。C的字符串没有string类那么丰富的成员函数，这个不是问题，我们可以自己写，这也是我不想介绍string成员函数的原因，与其花时间去研究string的成员函数，还不如自己写一个。

所以，了解string类的原理和一些用得着的成员函数就可以了，不必太深入研究，意义不大。
