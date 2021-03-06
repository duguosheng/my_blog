---
title: C++面向对象
date: 2019-11-09 11:19:47
tags:
- 面向对象
categories:
- C/C++
description:
- C++是一门面向对象的语言
---

<!--more-->
转自[码农有道](https://blog.csdn.net/wucz122140729/article/details/98477538)

### C++ 类和对象
C语言中结构体（struct）是一种构造类型，可以包含若干成员变量，可以通过结构体来定义结构体变量。

C++中的类（class）可以看成结构体的升级版，类也是一种构造类型，但是进行了一些扩展，类的成员不但可以有变量，还可以有函数，通过类定义出来的变量也有特定的称呼，叫做对象。

* 示例
```C++
#include <stdio.h>
#include <string.h>

class Girl
{
    public:
        char name[50];
        int age;
        double height;

        void Show();
};

void Girl::Show()
{
    printf("姓名：%s，年龄：%d， 身高：%lf\n",\
            name, age, height);
}

int main()
{
    Girl girl;
    strcpy(girl.name, "Mary");
    girl.age = 20;
    girl.height = 168.4;
    girl.Show();
}
```

> 运行结果
![面向对象](o1.png)

class是C++的关键字，用于定义类，就像结构体中的sturct。
public也是C++中的关键字，用于指定权限。
C语言中的 struct 只能包含成员变量，而C++中的 class 除了可以包含成员变量，还可以包含成员函数，例如Show()。在C语言中，结构体和函数是分离的，通过函数的参数传递变量；而在C++中，成员变量和成员函数都放在class内部声明，是聚集在一起的，看起来更像一个整体，成员函数可以直接访问成员变量，不必传递参数。

类的成员变量和普通变量一样，也有数据类型和名称。与结构体一样，在定义类的时候也不能对成员变量赋值。

`int CGirl::Show()`是类的成员函数的定义语法，在函数前加上类的名称和两个冒号，表示该函数是这个类的成员函数，函数的返回值、参数等语法和使用方法与C语言的普通函数相同。

在C++中，用类定义一个类变量叫做创建（或实例化）一个对象；某些资料中，把成员变量称为属性（property）；成员函数称为方法（method）。在我的教程中，将采用类、创建对象、成员变量和成员函数的称呼。

创建对象以后，可以使用点号.来访问成员变量和成员函数，这和通过结构体变量来访问它的成员一样。类的成员变量和成员函数的作用域和生命周期由对象的作用域和生命周期决定。

### 对象数组
对象可以被定义成数组对象，本质上与其它类型的数组变量没有区别。
```C++
CGirl Girl[10];
strcpy(Girl [0].m_name,”杨玉环”);
Girl [0].m_age=18;

……
```

在实际开发中，我们很少用对象数组。

### 对象的指针
类是一种自定义的数据类型，对象也是内存变量，也有内存地址，当然也就有了类的指针。

在指针章节中我们已经学习过，采用不同数据类型的指针指向不同数据类型的变量的地址，这一规则也适用于对象。如下：
```C++
CGirl queen;
CGirl *pst=& queen;
//通过类指针可以访问对象的成员，一般形式为：
(*pointer).memberName
//或者：
pointer->memberName
```

第一种写法中，圆点.的优先级高于`*`，`(*pointer)`两边的括号不能少。如果去掉括号写作`*pointer.memberName`，那么就等效于`*(pointer.memberName)`，这样意义就完全不对了。

第二种写法中，->是一个新的运算符，习惯称它为“箭头”，有了它，可以通过对象的指针直接访问对象的成员。

上面的两种写法是等效的，我们通常采用后面的写法，这样更加直观。

### 对象作为函数的参数
与结构体一样，对象可以作为函数参数传递，最好的办法也是传递对象的地址。

### 对象的初始化和占用内存的大小
按我们以前的经验，定义的变量使用前要初始化，C语言的基本数据类型可以直接赋值0，字符串和结构体用memset函数初始化，那么类的对象呢？对象不能用memset初始化，具体做法我们以后再介绍。

对象可以用sizeof运算符获取占用内存的大小。

### 小结
在这个阶段，类就是一个有成员函数的结构体，定义的关键字和语法不同，使用方法完全相同。

各位可能会认为类好像没什么用，不用类也可以活得很好，这不一定，因为我现在只是用尽可能简单的方式介绍类的相关知识，在接下来的章节中也是如此，如果我把实际开发的方法搬到教材中，大家可能接受不了，在实际开发中，某些类的代码非常长，类的定义就有好几页，还不包括成员函数体。希望各位保持好的心态，循序渐进的学习。

### 面向对象编程（Object Oriented Programming，OOP）
类是一个通用的概念，C++、Java、C#、PHP 等很多编程语言中都支持类，都可以通过类创建对象。因为 C++、Java、C#、PHP 等语言都支持类和对象，所以使用这些语言编写程序也被称为面向对象编程，这些语言也被称为面向对象的编程语言。C语言因为不支持类和对象的概念，被称为面向过程的编程语言。

在C语言中，我们会把重复使用或具有某项功能的代码封装成一个函数，而在C++中，多了一层封装，就是类（class），不要小看类（class）这一层封装，它有很多特性，极大地方便了程序员的开发，也让C++成为面向对象的语言。

面向对象编程在代码执行效率上绝对没有任何优势，它的主要目的是方便程序员组织和管理代码，快速梳理编程思路，带来编程思想上的革新。
