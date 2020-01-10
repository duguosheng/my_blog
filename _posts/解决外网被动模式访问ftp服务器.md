---
title: 解决外网被动模式访问ftp服务器
date: 2020-01-10 20:42:42
tags:
- ftp
categories:
- problems
---

配置完成ftp服务器后可以连接，执行cd等命令，但不能从服务器获取数据，我的原因是因为服务器返回了本地的地址，而不是公网的ip地址
<!--more-->
解决方法
```bash
sudo vim /etc/vsftpd/vsftpd.conf
```

修改或添加以下参数
```
listen=YES
listen_ipv6=NO
pasv_enable=YES
pasv_address=服务器公网ip
pasv_min_port=5500
pasv_max_port=6000
```

放行5500-6000端口为tcp方式，如果是买的如阿里云服务器，去控制台也放行一下，这样就可以了
