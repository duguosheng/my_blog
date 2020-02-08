---
title: ftp服务搭建
date: 2020-01-08 12:35:09
tags:
- ftp
categories:
- linux
description:
---

参考文章[https://blog.csdn.net/langyue919/article/details/80796369](https://blog.csdn.net/langyue919/article/details/80796369)

<!--more-->

* 默认在root用户下操作
### 安装服务器
```bash
yum install -y vsftpd
```

### 配置端口
```bash
# tcp模式放行20, 21端口
firewall-cmd --zone=public --add-port=21/tcp --permanent
firewall-cmd --zone=public --add-port=20/tcp --permanent
```

* 同时各个云服务器要在网页控制台直接放行端口，或配置安全组策略

### 修改配置文件
```bash
cd /etc/vsftpd
vi vsftpd.conf
```

* 修改以下参数
```
listen=YES
listen_ipv6=NO
```

* 添加如下参数
```
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES

pasv_enable=YES
pasv_address=123.1.1.1  #服务器外网ip地址
pasv_min_port=5500
pasv_max_port=6000
```

### 配置被动模式所用端口
* 上面配置文件中选定了5500-6000端口，现在放行
```bash
firewall-cmd --zone=public --add-port=5500-6000/tcp --permanent
```

### 关闭selinux
```bash
vi /etc/selinux/config
```

* 修改
```
selinux=disable
```

* 执行`setenforce 0`立即生效

### 创建ftp用户
```bash
vim /etc/vsftpd/vsftpd_login
```

* 写入虚拟用户的用户名和密码：
```
user1          奇数行  写用户名
123            偶数行  写密码
user2          多个用户就写多组，中间不能有空行和空格
456
```

* 更改权限：
```bash
chmod 600 /etc/vsftpd/vsftpd_login
```
 

* 将文本文件转换成二进制文件：
```bash
db_load -T -t hash -f /etc/vsftpd/vsftpd_login /etc/vsftpd/vsftpd_login.db
```
 

* 创建虚拟用户的配置文件目录：
```bash
mkdir /etc/vsftpd/vsftpd_user_conf
cd /etc/vsftpd/vsftpd_user_conf
```

* 创建虚拟用户的配置文件：
```bash
vim user1       #此处的文件名必须和/etc/vsftpd/vsftpd_login文件里的用户名一致
```

* 写入以下内容
```
#定义虚拟用户的家目录
local_root=/home/user1

#不允许匿名用户登陆，如果允许则用YES
anonymous_enable=YES

#写权限，允许
write_enable=YES

#设定umask，用来控制用户创建文件和目录的默认权限
local_umask=022

#不允许匿名用户上传
anon_upload_enable=NO

#不允许匿名用户创建目录和写权限
anon_mkdir_write_enable=NO

#空闲时限600秒，超时自动断开
#idle_session_timeout=600

#数据连接（请求）时限120秒，超时会自动断开
#data_connection_timeout=120

#客户端的最大连接数
max_clients=10
pasv_promiscuous=YES
```

* 定义用户验证文件路径
```
vim /etc/pam.d/vsftpd    #编辑ftp的用户认证文件
#在最前面加上：注意区分系统是32位还是64位
#指定密码验证形式为文件形式，并指定路径路径
auth sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/vsftpd_login 
#指定账户存储形式为文件，并指定账户存储文件路径
account sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/vsftpd_login
```

### 启动vsftpd服务
```
systemctl start vsftpd
```

---

**配置完成**，接下来就可使用ftp客户端连接了
