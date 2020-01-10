---
title: 'centos7更新gcc,并解决version ''GLIBCXX_3.4.21'' not found'
date: 2020-01-09 19:47:23
tags:
- gcc
- centos
categories:
- problems
description:
- centos中自带的gcc版本过低
---

<!--more-->
1. 升级gcc
[https://www.cnblogs.com/NanZhiHan/p/11010130.html](https://www.cnblogs.com/NanZhiHan/p/11010130.html)

2. 解决解决version ''GLIBCXX_3.4.21'' not found'
安装完成新版本gcc后，根据上面的教程安装在了`/usr/local/gcc`下，
首先
```bash
cd /usr/local/gcc/lib64
# 根据升级的gcc版本不同，这里的文件名可能后面的某些数字不同
sudo cp libstdc++.so.6.0.25 /usr/lib64
cd /usr/lib64
# 如果已经存在则将其备份
sudo mv libstdc++.so.6 libstdc++.so.6.back
# 将拷贝进来的文件建立软连接
sudo ln -s libstdc++.so.6.0.25 libstdc++.so.6
```

