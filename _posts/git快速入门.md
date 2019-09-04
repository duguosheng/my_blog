---
title: git快速入门
date: 2019-08-29 19:32:23
tags:
    - git
    - 计算机
categories:
    - git
---


* `git init`
> 将当前文件下变为git仓库

<!--more-->

* `git status`
> 查看git仓库的状态

* `git add`
```python
git add 文件  #添加指定文件的修改
git add .     #添加所有文件的修改
```

* `git diff` 
> 显示文件修改信息

* `git reset`
> 退回追踪

* `git config` 
```python
git config --global user.name "zhangsan"            #配置用户名
git config --global user.email "example@qq.com"     #配置邮箱
git config --global core.editor vim                 #配置默认编辑器
git config credential.helper store                  #在执行`push`前输入这段命令，会使git记住用户名和密码，之后就不用再输入
```

* `git commit` 
```python
git commit                 #会打开编辑器，可输入描述信息
git commit -m "some text"  #提交更改并添加描述信息
```

* 让git忽略管理某些文件
```python
vim .gitignore   #创建此文件，并在其中编辑要忽略的文件名即可

```

> 在该列表中添加的文件必须是从未被追踪过的，否则git将继续追踪该文件

* `git rm` 
```python
git rm --cached 文件    #让git停止追踪该文件
```

* 分支
```python
git branch          #显示分支列表
git branch test     #创建一个名为test的分支
git checkout test   #切换到test分支
git merge test      #将test分支添加到master
git branch -d test  #删除test分支，未添加的分支不可删除
git branch -D test  #强制删除test分支
```

* 推到github
> 首先去github创建一个新的仓库，并复制链接，假设为`https://.../abc.git` 
```python
git remote add origin https://.../abc.git       #告诉git你在网上的仓库位置
git push --set-upstream origin master           #将master提交到github（需要输入用户名和密码）
```

* 发送合作邀请
> 在github的仓库中`setting`-->`Collaborators`-->搜索用户-->点击发送邀请至邮箱

* 复制文件
```python
git clone https://.../abc.git   #复制仓库至本地，`.gitignore`中的文件不会被下载
```

* git pull
将github上更改过的文件下载到本地
