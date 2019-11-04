---
title: polybar主题配置
date: 2019-10-04 10:40:19
tags:
- linux
- polybar
categories:
- linux
description: polybar是一款强大的状态栏应用
---

<!--more-->

> 折腾了一天的`polybar`主题，来记录一下

## 主题下载

* 首先，你已经下好了polybar
* 然后下载`polybar`的主题
```bash
cd ~/.config
git clone https://github.com/adi1090x/polybar-themes.git
```

> 然后在`.config`目录下就会有一个名为`polybar-themes`的文件夹


## 配置字体

* 就会看到一共九个主题，每个主题下都有一个名为`fonts`的文件夹，想要使用哪个主题，先将主题的字体复制到你系统的字体文件夹下
> 这个字体我搞了好长时间(大概一下午加晚上)，一直以为要自己下载，下载了还显示不出来，原来他自带，我....
```bash
cd polybar-themes
ls -l
```

以`polybar-6`为例，它的字体目录如下

![字体](p1.png)

我的字体目录是`/usr/share/fonts/`，我的`ttf`尾缀的字体在`/usr/share/fonts/TTF/`下，所以我将`ttf`字体复制到这里面，然后`termsyn`是个字体文件夹，直接放在`fonts`目录下即可，`siji`放在`/usr/share/fonts/misc/`下

> 最后将原来的`~/.config/polybar/`这个目录改个名字，如果不需备份的话直接删除就好了，然后我用的`polybar-5`

```bash
cp -r ~/.config/polybar-themes/polybar-5 ~/.config/polybar
```

> 接下来运行脚本
```bash
cd ~/.config/polybar/
./launsh.sh
```

## 配置(polybar-5)

* 如果想更改标题栏顺序，可以在主题中`config.ini`中修改
建议将`[bar/top]`下
```
modules-left = menu title right-end-top left-end-bottom workspaces right-end-top left-end-bottom colors-switch right-end-top
```

改为
```
modules-left = menu workspaces right-end-top left-end-bottom colors-switch right-end-top left-end-bottom title right-end-top
```

用起来感觉好些


* 如果要更改左上角菜单栏中`Files`，`Terminal`等的程序，可以在主题下`user_modules.ini`中`[module/menu]`下更改

* 解决polybar显示不了中文
打开主题目录下`config.ini`，将
```
font-0 = Iosevka Nerd Font:style=Medium:size=14;3
```
改为
```
font-0 = unifont:style=Medium:size=14;3
```

> 当然也可以下载其他支持中文的字体

* 我用的`i3wm`所以它的右上角`powermenu`菜单中`logout`用不了

首先下载`i3exit`
```bash
sudo pacman -S i3exit
```

然后更改主题文件夹下`scripts/powermenu`，将
```bash
*Logout) openbox --exit ;;
```

改为
```bash
*Logout) i3exit logout ;;
```


