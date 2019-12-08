---
title: QT多线程
date: 2019-12-03 23:54:12
tags:
- 多线程
categories:
- QT
description:
- QT中实现多线程的方式
---

<!--more-->
## QT4
* 自定义一个类，继承自**QThread**，并实现**run()**方法
```C++
class MyThread: public Qthread
{
public:
	void run();  //线程处理函数（和主线程不在同一个线程）
signals:
	void isDone();  //用于判断线程是否结束
}

void MyThread::run()
{
	QThread.sleep(5);  //sleep延时模拟实际工程中的处理函数
	emit isDone();  //处理完触发isDone()
}
```

* 创建对象并调用**start()**方法
```C++
MyThread th;
th.start();
```

* 关闭线程
```C++
//关闭线程
th->quit();
//等待线程处理完手头工作
th->wait();
```

## QT5
* 自定义一个类，继承自**QObject**
* 类中设置一个线程函数（只有一个是线程函数）
* 在主线程中创建线程对象（不能指定父对象）myTh和Qthread实例对象th
* 使用`myTh->moveToThread(th)`把自定义线程类加入到子线程
* 使用connect建立一定关系的连接
* 调用Qthread实例对象的`start()`方法启动线程

> mythread.h
```C++
#ifndef MYTHREAD_H
#define MYTHREAD_H

#include <QObject>
#include <QThread>
#include <QDebug>

class MyThread : public QObject
{
    Q_OBJECT
public:
    explicit MyThread(QObject *parent = 0);
    void doWork();  //线程处理函数
    void setFlag(bool flg=false);  //标志位，通过设置标志位进入或退出处理函数的循环

signals:
    void mySignal();

private:
    bool flag;

public slots:
};

#endif // MYTHREAD_H
```

> mythread.cpp
```C++
#include "mythread.h"

MyThread::MyThread(QObject *parent) : QObject(parent)
{

}

void MyThread::setFlag(bool flg)
{
    flag = flg;
}

void MyThread::doWork()
{
    while(flag)
    {
        QThread::sleep(1);  //每隔一秒触发一次信号
        emit mySignal();
        qDebug() << "子线程号：" << QThread::currentThread();
    }
}
```

> widget.h
```C++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QThread>
#include "mythread.h"
#include <QDebug>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();
    QThread *th;
    MyThread *myTh;

signals:
    void startThread();

private slots:
    void on_start_button_clicked();
    void on_stop_btn_clicked();
    void addLcdNum();
    void dealClose();

private:
    Ui::Widget *ui;
};

#endif // WIDGET_H
```

> widget.cpp
```C++
#include "widget.h"
#include "ui_widget.h"


Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //1. 为线程分配空间
    //创建QThread对象
    th = new QThread(this);
    //创建自定义线程类对象，该类必须继承于QObject
    myTh = new MyThread;

    //2. 将自定义线程加入到QThread创建的对象中
    myTh -> moveToThread(th);

    //当触发自定义线程中的信号时，使LCD显示的数字增加
    connect(myTh, &MyThread::mySignal, this, &Widget::addLcdNum);
    //3. 当触发主界面类的开启线程信号时，开启线程处理函数,调用th->start()
    connect(this, &Widget::startThread, myTh, &MyThread::doWork);
    //当窗口被关闭时，释放资源
    connect(this ,&Widget::destroyed, this, &Widget::dealClose);

    qDebug() << "主线程是：" << QThread::currentThread();
}

Widget::~Widget()
{
    delete ui;
}

void Widget::addLcdNum()
{
    static int num = 0;
    ui -> lcdNumber -> display(++num);
}

//按下开始键，触发开启线程的信号
void Widget::on_start_button_clicked()
{
    if(th->isRunning())
        return;
    th -> start();
    myTh -> setFlag(true);
    emit startThread();  //触发信号
}

//按下停止键，将标志位翻转
void Widget::on_stop_btn_clicked()
{
    if(!(th->isRunning()))
        return;
    myTh -> setFlag(false);
    th -> quit();
    th -> wait();
}

//关闭窗口
void Widget::dealClose()
{
    on_stop_btn_clicked();
    delete myTh;
}
```