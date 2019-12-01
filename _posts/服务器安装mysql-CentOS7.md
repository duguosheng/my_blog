---
title: 服务器安装mysql(CentOS7)
date: 2019-11-29 17:22:08
tags:
- mysql
- 服务器
categories:
- problems
description:
- 折腾了半天安装mysql
---

<!--more-->

### 安装Yum资源包
CentOS 7 版本中 MySQL数据库已从默认的程序列表中移除，所以在安装前我们需要先去官网下载 Yum 资源包，下载地址为：![官网链接](https://dev.mysql.com/downloads/repo/yum/)
![下载](f1.png)
点击Download下载到本地
然后
```bash
#linux下使用scp命令将下载的文件传输到服务器，windows下可以使用xftp软件
scp /path/to/download/package user@123.31.31.45:path/to/your/server
```

### 开始安装
```bash
sudo wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
cd 放置刚才下载.rpm文件的目录
sudo rpm -Uvh mysql80-community-release-el7-3.noarch.rpm
sudo yum update
```

通过修改vim /etc/yum.repos.d/mysql-community.repo文件，改变默认安装的mysql版本。比如要安装5.6版本，将5.7源的enabled=1改成enabled=0，然后再将5.6源的enabled=0改成enabled=1即可。
注意： 任何时候，只能启用一个版本。
查看当前的启用的 MySQL 版本：yum repolist enabled | grep mysql

### 安装
```bash
yum install mysql-server -y
```

### 初始化mysql
```bash
sudo mysqld --initialize
```

### 启动
```bash
sudo systemctl start mysqld
```

然后这一步我就出问题了，我把`/var/lib/mysql删除后就可以了`
