---
title: archlinux包管理工具pacman
date: 2019-08-29 16:54:50
tags:
    - pacman
    - linux
categories:
    - linux
---


pacman的基本命令分为三大类S,R,Q

<!--more-->

#### command with **`S`** (means synchronized<同步的>)
| command                     | function                                                   | example             |
|-----------------------------|------------------------------------------------------------|---------------------|
| sudo pacman -S (some apps)  | install software                                           | sudo pacman -S vlc  |
| sudo pacman -Sy             | only synchronize source(仅同步源)                          |                     |
| sudo pacman -Syy            | force to refresh for updates(强制刷新一遍更新信息)         |                     |
| sudo pacman -Su             | update the system                                          |                     |
| sudo pacman -Syu            | synchronize source and update                              |                     |
| sudo pacman -Syyu           | force to refresh imformation for updates and update system |                     |
| sudo pacman -Ss (some apps) | search for the software from internet                      | sudo pacman -Ss vim |
| sudo pacman -Sc             | delete software installation packages                      |                     |


#### command with `R` (means remove)
| command                 | function                                                              | example              |
|-------------------------|-----------------------------------------------------------------------|----------------------|
| sudo pacman -R (app)    | delete the software                                                   | sudo pacman -R vim   |
| sudo pacman -Rs (app)   | delete the software and the packages it rely on(删除软件及其依赖的包) | sudo pacman -Rs vim  |
| *sudo pacman -Rns (app) | delete software, packages, and global profile                         | sudo pacman -Rns vim |


#### command with **`Q`** (means query<查询>)
| command                      | function                                      | example             |
|------------------------------|-----------------------------------------------|---------------------|
| sudo pacman -Q               | list all softwares be installed               |                     |
| sudo pacman -Qe              | list the software have been installed by user |                     |
| sudo pacman -Q &#124; wc -l  | show total number of softwares                |                     |
| sudo pacman -Qe &#124; wc -l | show number of personal softwares             |                     |
| sudo pacman -Qeq             | list pesonal softwares without version        |                     |
| sudo pacman -Qs (app)        | query the softwares which contains letters    | sudo pacman -Qs vim |
| sudo pacman -Qdt             | query unrequired packages                     |                     |
| sudo pacman -Qdtq            | query unrequired packages(不需要的包)


#### combination commands
| command                        | function                   | example |
|--------------------------------|----------------------------|---------|
| sudo pacman -R $(pacman -Qdtq) | delete unrequired packages |         |







