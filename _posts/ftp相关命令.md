---
title: ftp相关命令
date: 2020-01-08 11:06:30
tags:
- ftp
categories:
- C/C++
description:
- ftp是用于传输文件的协议
---

<!--more-->

## 登录
```bash
# ftp接地址直接登录
ftp ip地址
# 先启动ftp然后使用open
ftp
open ip地址
```

之后输入用户名和密码即可

## 文件相关指令
```bash
dir     #显示服务器目录和文件列表
ls      #显示服务器目录和文件列表
cd      #进入服务器指定目录
lcd     #进入本地客户端指定目录
ascii   #设定传输方式为ascii传输
binary  #设定传输方式为二进制传输
get/recv oldname [newname] #下载服务器上的文件到本地，[并重命名为newname]
mget file1 file2 ...  #下载多个文件，支持通配符
put/send oldname [newname] #上传文件到服务器，[并重命名为newname]
mput file1 file2 ...  #上传多个文件，支持通配符
rename oldname newname #重命名文件
delete file  #删除文件
mdelete file1 file2 ...  #删除多个文件
help   #帮助信息
prompt #打开/关闭交互提示
bye    #退出ftp
passive  #主动与被动模式切换
```


