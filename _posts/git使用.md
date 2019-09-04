---
title: git使用
date: 2019-08-30 08:03:04
tags:
- 版本控制
- 代码管理
categories:
- git
---

## git版本创建

<!--more-->

```C
git init        //初始化git仓库
git add .       //添加全部修改
git add 文件名  //添加指定文件的修改
git commit -m "说明信息"    //提交并添加备注
```


## 查看信息
```C
git log     //查看版本信息
```
![git log](git_log.png) 

```C
git status  //查看修改信息
```
![gitstatus](git_status.png) 

## 版本回退

* `HEAD`指针会指向最新的版本
* `HEAD^`指向上一个版本
* `HEAD^^`指向上上个版本...
* 或者使用`HEAD~n`代表前n个版本

```C
git reset --hard HEAD^      //回退到上一个版本
git reset --hard HEAD~3     //回退到之前3个版本

```




