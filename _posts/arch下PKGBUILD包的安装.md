---
title: arch下PKGBUILD包的安装
date: 2019-10-04 17:04:10
tags:
- PKGBUILD
categories:
- linux
description: arch下另一种安装软件的方式
---

<!--more-->

* cd到有`PKGBUILD`文件的目录下
```bash
# 生成后缀.pkg.tar.xz的压缩文件
makepkg
# 使用pacman安装
sudo pacman -U *.pkg.tar.xz
```


