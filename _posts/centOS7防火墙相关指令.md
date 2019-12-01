---
title: centOS7防火墙相关指令
date: 2019-11-16 00:28:54
tags:
- 防火墙
categories:
- linux
---

转载自[Centos7查看防火墙对应的开放端口](https://blog.csdn.net/m0_37779570/article/details/81979073)

<!--more-->
### 查看已经开放的端口
```bash
firewall-cmd --list-ports 
```

### 开启端口
```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent  
```

* 命令含义:
–zone #作用域
–add-port=80/tcp #添加端口，格式为：端口/通讯协议
–permanent #永久生效，没有此参数重启后失效
* 开启端口后记得重启
```bash
firewall -cmd --reload
```

### 防火墙其他命令

* 停止firewall  
```bash
systemctl stop firewalld.service  
```
* 禁止firewall开机启动  
```bash
systemctl disable firewalld.service
```
