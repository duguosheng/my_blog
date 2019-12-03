---
title: QT学习1
date: 2019-12-01 16:56:47
tags:
- QT
categories:
- C/C++
description:
- QT是一款跨平台的图形界面引擎
---

<!--more-->
创建工程时，会看到如下界面
![工程](q1.png)
其实，`QMainWindows`和`QDialog`是`QWidget`的子类，`QWidget`只有一个空界面，而另外两个子类在其基础上增添了其他功能


### 新建源文件的注释说明
```C++
#include "mywidget.h"
#include <QApplication>  //包含一个应用程序类的头文件

//main程序入口
int main(int argc, char *argv[])
{
    //a应用程序对象，在Qt中，应用程序对象有切纸有一个
    QApplication a(argc, argv);
    //窗口对象，myWidget父类->QWidget
    myWidget w;
    //窗口对象，需使用show方法来显示
    w.show();

    //让应用程序对象进入消息循环
    //让代码阻塞到这行
    return a.exec();
}
```

### 新建源文件的注释说明
```C++
#ifndef MYWIDGET_H
#define MYWIDGET_H

#include <QWidget>

class myWidget : public QWidget
{
    //这是一个宏，允许类中使用信号和槽的机制
    Q_OBJECT

public:
    myWidget(QWidget *parent = nullptr);
    ~myWidget();
};
#endif // MYWIDGET_H
```


### QT的pro文件
```py
# QT包含的模块
QT       += core gui

# 大于QT4版本以上，包含widget模块
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

# 目标：生成的.exe程序的名称
TARGET = 01_FirstProject

# 模板：应用程序模板Application
TEMPLATE = app

# 源文件
SOURCES += \
    main.cpp \
    mywidget.cpp

# 头文件
HEADERS += \
    mywidget.h

```

### 一些快捷键
| 快捷键            | 功能                   |
|-------------------|------------------------|
| ctrl+/            | 注释                   |
| ctrl+r            | 运行                   |
| ctrl+b            | 编译                   |
| ctrl+滚轮         | 字体缩放               |
| ctrl+f            | 查找                   |
| ctrl+shift+上下键 | 整行移动               |
| ctrl+i            | 自动对齐               |
| <F4>              | 同名的.h和.cpp文件切换 |
| <F1>              | 帮助文档               |

