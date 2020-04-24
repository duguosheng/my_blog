---
title: C++类的继承
date: 2019-11-12 21:50:29
tags:
- 继承
categories:
- C/C++
description:
- 通过继承父类来缩减重复的代码
---

<!--more-->
### 继承
继承是面向对象程序设计中最重要的一个概念。继承允许我们根据一个类来定义另一个类，达到了代码功能重用效果。
当创建一个类时，如果待创建的类与另一个类存在某些共同特征，程序员不需要全部重新编写成员变量和成员函数，只需指定继承另一个类即可，被继承的类称为基类或父类，新建的类称为派生类或子类。

### 基类和派生类
定义一个派生类，需要指定它的基类，语法如下：
```C++
class derived-class: access-specifier base-class
```

derived-class：派生类名。
access-specifie：访问修饰符 access-specifier 是 public、protected 或 private 其中的一个，base-class 是之前定义过的某个类的名称。如果未使用访问修饰符 access-specifier，则默认为 private。
base-class：基类名

* 示例
```C++
#include <stdio.h>
#include <string>
using namespace std;

class Animal{
public:
    string m_name;
    int age;

    int shout(){
        printf("动物叫");
        return 0;
    }
};

class Cat: public Animal{
    public:
    char color[20];
};

int main()
{
    Cat *cat = new Cat();
    cat->shout();
}
```

> 运行结果如下：
```
动物叫
```

### 访问控制和继承
派生类可以访问基类中所有的非私有成员变量和成员函数，如果基类的成员变量和成员函数不想被派生类访问，则应在基类中声明为 private。

### 继承类型
当一个类派生自基类，该基类可以被继承为 public、protected 或 private 几种类型。继承类型是通过上面讲解的访问修饰符 access-specifier 来指定的。

我们几乎不使用 protected 或 private 继承，通常使用 public 继承。当使用不同类型的继承时，遵循以下几个规则：

* 公有继承（public）：当类派生以public方式继承时，基类的公有成员也是派生类的公有成员，基类的保护成员也是派生类的保护成员，基类的私有成员不能直接被派生类访问，但是可以通过调用基类的公有和保护成员来访问。

* 保护继承（protected）： 当类派生以protected方式继承时，基类的公有和保护成员将成为派生类的保护成员。

* 私有继承（private）：当类派生以private方式继承时，基类的公有和保护成员将成为派生类的私有成员。

### 基类与派生类的指针
基类的指针可以指向基类对象。
派生类的指针可以指向派生类对象。
基类的指针可以指向派生类对象，但是不能通过基类的指针访问派生类的成员。
派生类的指针不可以指向基类对象。

### 多继承
多继承即一个派生类可以有多个基类，它继承了多个基类的特性。
C++ 类可以从多个基类继承成员，语法如下：
```C++
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
    // 派生类类体
};

```

其中，访问修饰符继承方式是 public、protected 或 private 其中的一个，用来修饰每个基类，各个基类之间用逗号分隔。
