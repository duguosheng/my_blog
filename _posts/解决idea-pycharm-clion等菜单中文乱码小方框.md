---
title: 'manjaro解决idea,pycharm,clion等菜单中文乱码小方框'
date: 2019-09-26 10:54:55
tags:
- idea乱码
categories:
- IDE
description: 当Jetbrains菜单中包含中文描述(如安装插件时的描述信息)
---

<!--more-->

> 这是由于字体不支持中文造成的
* 解决方法
> 终端下载字体
```bash
sudo pacman -S wqy-microhei
```
> 在ide设置`Appearance->Theme下勾选use custom font->选择WenQuanYi micro hei`更改字体
![更改字体](jet_1.png)

> 接下来就可以看到中文了
![更改字体](jet_2.png)



