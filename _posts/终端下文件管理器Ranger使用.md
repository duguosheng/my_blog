---
title: 终端下文件管理器Ranger使用
date: 2019-08-28 12:54:08
tags:
    - 文件管理
    - 计算机
categories:
    - linux
---

## 选择/打开文件

<!--more-->

| 命令         | 功能                             |
|--------------|----------------------------------|
| `left`或`h`  | 上一级                           |
| `right`或`l` | 下一级/打开文件                  |
| `up`或`k`    | 上一个                           |
| `down`或`j`  | 下一个                           |
| `gg`         | 最上方                           |
| `G`          | 最下方                           |
| `r`          | 选择打开方式                     |
| `[`          | 移动至上一个父文件夹             |
| `]`          | 移动至下一个父文件夹             |
| `H`          | 退回上一个位置                   |
| `L`          | 撤销退回                         |
| `<Space>`    | 选择该文件，可多选               |
| `v`          | 反转选择                         |
| `V`          | 进入可视模式，结合移动键进行选择 |

* 打开文件默认使用`nano`或者`gedit`，如果想要修改，
* 查看默认编辑器
```shell
echo $EDITOR
```

* 修改默认编辑器(如改为vim)
> 如果使用`bash`

```shell
export EDITOR="/usr/bin/vim"
```

> 如果使用`fish` 
```shell
set -g -x EDITOR "/usr/bin/vim"
```


## 复制粘贴

| 命令 | 功能                                       |
|------|--------------------------------------------|
| `y`  | 按照提示选择复制的内容，如文件，文件路径等 |
| `yy` | 复制一个文件                               |
| `p`  | 按照提示招贴                               |
| `pp` | 复制刚才粘贴的，不覆盖                     |
| `po` | 复制并覆盖重名文件                         |
| `dd` | 剪切                                       |
| `dD` | 删除                                       |
| `dU` | 查看文件大小                               |

* 复制一个很大的文件进行粘贴时，可使用`w`进入**进度管理**
* 在**进度管理**中，`dd`取消当前任务

## 其他操作

| 命令                           | 功能                                       |
|--------------------------------|--------------------------------------------|
| `/搜索内容`                    | 查找文件,`n`下一个,`N`上一个               |
| `f 搜索内容`                   | 查找文件，并直接指向该文件                 |
| `zh` / `<BackSpace>` / `<C-h>` | 显示/隐藏隐藏文件                          |
| `cw` / `a` / `i` / `A` / `I`   | 重命名文件                                 |
| `o`                            | 按照提示选择排序方式                       |

## 生成Ranger配置文件
* 在终端下执行

```python
ranger --copy-config=all 
```

就会在`.config/ranger/`下看到相关配置文件，更多配置信息可以去[ranger的github下的wiki栏目](https://github.com/ranger/ranger/wiki)下查看
