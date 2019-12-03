---
title: QT中信号和槽
date: 2019-12-02 10:50:52
tags:
- 信号与槽
categories:
- QT
description:
- 信号槽是 Qt 框架引以为豪的机制之一
---

<!--more-->
### 概述
信号槽是 Qt 框架引以为豪的机制之一。所谓信号槽，实际就是观察者模式。当某个事件发生之后，比如，按钮检测到自己被点击了一下，它就会发出一个信号（signal）。这种发出是没有目的的，类似广播。如果有对象对这个信号感兴趣，它就会使用连接（connect）函数，意思是，将想要处理的信号和自己的一个函数（称为槽（slot））绑定来处理这个信号。也就是说，当信号发出时，被连接的槽函数会自动被回调。这就类似观察者模式：当发生了感兴趣的事件，某一个操作就会被自动触发。（这里提一句，Qt 的信号槽使用了额外的处理来实现，并不是 GoF 经典的观察者模式的实现方式。）

信号和槽是Qt特有的信息传输机制，是Qt设计程序的重要基础，它可以让互不干扰的对象建立一种联系。

### connect函数
connect()函数最常用的一般形式：
```C++
connect(sender, signal, receiver, slot);
```

参数：
sender：发出信号的对象
signal：发送对象发出的信号(地址)
receiver：接收信号的对象
slot：接收对象在接收到信号之后所需要调用的函数(地址)

信号槽要求信号和槽的参数一致，所谓一致，是参数类型一致。如果不一致，允许的情况是，槽函数的参数可以比信号的少，即便如此，槽函数存在的那些参数的顺序也必须和信号的前面几个一致起来。这是因为，你可以在槽函数中选择忽略信号传来的数据（也就是槽函数的参数比信号的少），但是不能说信号根本没有这个数据，你就要在槽函数中使用（就是槽函数的参数比信号的多，这是不允许的）。

### 示例
在使用widget新建的工程中，在`widget.cpp`的构造函数中写入下面的函数
```C++
#include "widget.h"
#include <QPushButton.h>

Widget::Widget(QWidget *parent)
    : QWidget(parent)
{
    QPushButton *btn1 = new QPushButton();
//    btn1->show();  //show会以顶层方式弹出窗口控件
    btn1->setParent(this);
    btn1->setText("关闭窗口");
    btn1->move(100, 100);
    setFixedSize(600, 400);  //设置窗口固定大小
    //调用connect实现信号与槽函数的连接
    connect(btn1, &QPushButton::clicked, this, &Widget::close);
}
```

这样建立了点击按钮这个信号和关闭窗口这个槽函数的连接，当按下按钮时，窗口就会关闭


### 自定义信号与槽
* 实现的效果是->信号：下课-老师饿了  槽：学生请老师吃饭
* 首先创建两个类老师Teacher，学生Student
* 老师在signals下声明hungry()方法，不需实现
* 学生在public slots下声明treat()方法，需要实现，实现如下
```C++
void Student::treat()
{
    qDebug() << "请客吃饭";
}
```

* 在Widget类中声明并实现ClassIsOver()方法，使用emit触发Teacher类中hungry()信号
```C++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
    //创建老师，学生对象
    this->tc = new Teacher(this);
    this->st = new Student(this);

    //建立老师饿了，学生请客的链接
    connect(tc, &Teacher::hungry, st, &Student::treat);

    //调用下课函数，
    classISOver();
}

void Widget::classISOver()
{
    //下课函数,调用此函数后使用emit触发老师饿了的信号
    emit tc->hungry();
}

Widget::~Widget()
{
    delete ui;
}
```

* 如果发生重载，可以定义函数指针来指向确定的函数，如在上述例子中重载hungry和treat方法中加入QString参数，指明吃什么，就可以通过下面的写法指明
```C++
//定义函数指针，指向重载函数中确定的某一个
void (Teacher::*tcSignal)(QString) = &Teacher::hungry;
void (Student::*stSlot)(QString) = &Student::treat;
connect(tc, tcSignal, st, stSlot);
```

* 一些注意事项
- [ ] 信号：
```
自定义信号，写到signals下
返回值是void，只需要声明，不需要实现
可以有参数，可以重载
```

- [ ] 槽：
```
早期QT版本，必须写到public slots下，高版本可以写到public或全局下
返回值void，需要声明，也要实现
可以有参数，可以重载
```

### 断开信号与槽
* 使用`disconnect`方法，参数复制`connect`中的即可

### 其他
* 信号可以连接信号
* 一个信号可以连接多个槽
* 多个信号可以连接同一个槽
* 信号和槽函数的参数必须类型一一对应
* 信号的参数个数可以多于槽函数参数的个数，之前的参数类型要一一对应
