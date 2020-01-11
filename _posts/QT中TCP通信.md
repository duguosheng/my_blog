---
title: QT中TCP通信
date: 2019-12-07 10:57:51
tags:
- TCP
categories:
- QT
description:
- 整体流程大同小异，记录一下方便以后查看
---

<!--more-->


* 整体流程如下
![tcp](t1.png)

### 代码如下
* 在pro文件中加入
```
QT += network
CONFIG += c++11
```
#### _include.h
```C++
#ifndef _INCLUDE_H
#define _INCLUDE_H

#include <QObject>
#include <QWidget>
#include <QApplication>
#include <QTcpServer>  //监听套接字
#include <QTcpSocket>  //通信套接字
#include <QHostAddress>

#include "tcpserver.h"
#include "tcpclient.h"

#endif // _INCLUDE_H
```

#### tcpclient.h
```C++
#ifndef TCPCLIENT_H
#define TCPCLIENT_H

#include "_include.h"

namespace Ui {
class TcpClient;
}

class TcpClient : public QWidget
{
    Q_OBJECT

public:
    explicit TcpClient(QWidget *parent = 0);
    ~TcpClient();

private slots:
    void on_btn_connect_clicked();
    void on_btn_send_clicked();

    void on_btn_close_clicked();

private:
    Ui::TcpClient *ui;
    QTcpSocket *client_sock;
};

#endif // TCPCLIENT_H
```

#### tcpclient.cpp
```C++
#include "tcpclient.h"
#include "ui_tcpclient.h"

TcpClient::TcpClient(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::TcpClient)
{
    ui->setupUi(this);
    setWindowTitle("客户端");
    client_sock = NULL;

    //分配空间
    client_sock = new QTcpSocket(this);
    connect(client_sock, &QTcpSocket::connected,\
            [=](){
        ui->text_recv->setText("成功连接服务器");
    });

    connect(client_sock, &QTcpSocket::readyRead, \
            [=](){
                //获取对方发送的内容
                QByteArray array = client_sock->readAll();
                //追加到编辑区中
                ui->text_recv->append(array);
    });
}

TcpClient::~TcpClient()
{
    delete ui;
}

void TcpClient::on_btn_connect_clicked()
{
    //获取服务器端口和IP
    QString ip = ui->lineEdit_ip->text();
    quint16 port = ui->lineEdit_port->text().toInt();

    //主动和服务器连接
    client_sock->connectToHost(QHostAddress(ip), port);
}

void TcpClient::on_btn_send_clicked()
{
    if(client_sock==NULL)
        return;
    //获取编辑框内容
    QString str = ui->text_send->toPlainText();
    ui->text_send->clear();
    //给对方发送数据，使用的是tcpSocket
    client_sock->write(str.toUtf8().data());
}

void TcpClient::on_btn_close_clicked()
{
    if(client_sock==NULL)
        return;
    //主动和对方断开连接
    client_sock->disconnectFromHost();
    client_sock->close();
    client_sock = NULL;
}
```

#### tcpserver.h
```C++
#ifndef TCPSERVER_H
#define TCPSERVER_H

#include "_include.h"

class TcpServer : public QObject
{
    Q_OBJECT

private:
    QTcpServer *tcpServer;
    QTcpSocket *tcpSocket;
    QString client_ip;
    quint16 client_port;
    void InitServer();  //初始化服务器

public:
    explicit TcpServer(QObject *parent = 0);
    bool Write(QString data);
    QByteArray Read();
    void DisConnect();
    ~TcpServer();

signals:
    void SuccessConnected(QString ip, quint16 port);  //成功连接客户端
    void RecvMessage(QByteArray data);

public slots:
};

#endif // TCPSERVER_H
```

#### tcpserver.cpp
```C++
#include "tcpserver.h"

//宏定义监听端口
#define TCP_SERVER_LISTEN_PORT 5894


TcpServer::TcpServer(QObject *parent) : QObject(parent)
{    
    InitServer();
}


void TcpServer::InitServer()
{
    tcpServer = new QTcpServer(this);
    tcpSocket = NULL;
    tcpServer->listen(QHostAddress::Any, TCP_SERVER_LISTEN_PORT);
    connect(tcpServer, &QTcpServer::newConnection, \
            [=](){
                //取出建立好连接的套接字
                tcpSocket = tcpServer->nextPendingConnection();
                //获取对方的ip和端口
                QString client_ip = tcpSocket->peerAddress().toString();
                quint16 client_port = tcpSocket->peerPort();
                emit SuccessConnected(client_ip, client_port);

                //当获取了tcpSocket后,建立连接关系，读取接收到的消息
                connect(tcpSocket, &QTcpSocket::readyRead, \
                        [=](){
                            QByteArray array = tcpSocket->readAll();
                            emit RecvMessage(array);
                });
    });
}

bool TcpServer::Write(QString data)
{
    if(tcpSocket==NULL)
        return false;
    //给对方发送数据，使用的是tcpSocket
    if(tcpSocket->write(data.toUtf8().data())==-1)
        return false;
    return true;
}

void TcpServer::DisConnect()
{
    if(tcpSocket==NULL)
        return;
    //主动和客户端断开连接
    tcpSocket->disconnectFromHost();
    tcpSocket->close();
    tcpSocket = NULL;
}

TcpServer::~TcpServer()
{

}

```

#### widget.h
```C++
#ifndef WIDGET_H
#define WIDGET_H

#include "_include.h"

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();

private:
    Ui::Widget *ui;
    TcpServer *tcp_server;

private slots:
    void DealSuccessConnected(QString ip, quint16 port);
    void DealRecvMessage(QByteArray data);
    void on_button_send_clicked();
    void on_button_close_clicked();
};

#endif // WIDGET_H
```

#### widget.cpp
```C++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
    setWindowTitle("服务器：5894");
    tcp_server = new TcpServer(this);
    //连接成功后输出提示信息
    connect(tcp_server, &TcpServer::SuccessConnected, this, &Widget::DealSuccessConnected);
    //收到消息后显示在文本框
    connect(tcp_server, &TcpServer::RecvMessage, this, &Widget::DealRecvMessage);
}

void Widget::DealSuccessConnected(QString ip, quint16 port)
{
    QString info = QString ("%1:%2:成功连接").arg(ip).arg(port);
    ui->text_read->setText(info);
}

void Widget::DealRecvMessage(QByteArray data)
{
    ui->text_read->append(data);
}

void Widget::on_button_send_clicked()
{
    //获取编辑区内容
    QString str = ui->text_send->toPlainText();
    ui->text_send->clear();
    //发送数据
    tcp_server->Write(str);
}

void Widget::on_button_close_clicked()
{
    tcp_server->DisConnect();
}

Widget::~Widget()
{
    delete ui;
}
```



#### 说明

该程序会生成两个窗口（重合在一起，需用鼠标拖开），一个服务端，一个客户端，连接后即可通信