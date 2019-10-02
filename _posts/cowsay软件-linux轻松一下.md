---
title: cowsay软件--linux轻松一下
date: 2019-09-27 16:44:30
tags:
- cowsay
- 字符软件
categories:
- linux
description: cowsay是一款可以让动物说话的有趣软件
---

<!--more-->

### 基本用法
```bash
# cowsay 文本
cowsay hahaha
```

> 运行
![cow](cow.png)

### 使用其他动物
#### 罗列所有动物
```bash
cowsay -l
```

> 运行
```
Cow files in /usr/share/cows:
beavis.zen blowfish bong bud-frogs bunny cheese cower daemon default dragon
dragon-and-cow elephant elephant-in-snake eyes flaming-sheep ghostbusters
head-in hellokitty kiss kitty koala kosh luke-koala meow milk moofasa moose
mutilated ren satanic sheep skeleton small sodomized stegosaurus stimpy
supermilker surgery telebears three-eyes turkey turtle tux udder vader
vader-koala www
```

#### 切换动物
```bash
# 比如换成dragon-and-cow
cowsay -f dragon-and-cow hello
```

> 运行
![cow_dragon](cow_d.png)


