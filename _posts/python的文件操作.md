---
title: python的文件操作
date: 2019-09-09 19:56:50
tags:
- python进阶
categories:
- python
description: 在计算机中操作文件分为三步骤：打开，读写，关闭
---

<!--more-->

## 文件基本操作
### 操作文件的函数/方法

| 函数/方法 | 功能                     |
|-----------|--------------------------|
| open      | 打开文件，并返回操作对象 |
| read      | 将文件读取到内存         |
| write     | 将指定内容写入文件       |
| close     | 关闭文件                 |

> `read` `write` `close`三者都要通过**文件对象**来进行调用

### read方法
* `open()`第一个参数是要打开的文件名(区分大小写)
* 打开后操作完成要使用`close()`关闭，否则会消耗系统资源，影响后续对文件的访问
* 通常先编写打开和关闭文件代码，在编写中间的读写操作

```py
# 打开
file = open("文件名")
# 读取
text = file.read()
print(text)
# 关闭
file.close()
```

* 关于文件指针
  * python中读取完内容后会将指针移动到读取内容的末尾，默认是文件末尾
  * **连续两次读取文件，由于第一次读完指针已经在末尾，因而第二次读不到内容**

### 打开文件的方式
* `open()`默认以只读打开

```py
file = open("文件名", "访问方式")
```

| 访问方式 | 说明                                                 |
|----------|------------------------------------------------------|
| r        | 只读，指针在文件开头，不存在会抛出异常               |
| w        | 只写，若文件存在则覆盖，不存在则创建                 |
| a        | 追加，文件存在，指针就放在文件末尾，不存在，抛出异常 |
| r+       | 读写，文件指针在开头，若不存在，抛出异常             |
| w+       | 读写，文件存在则覆盖，不存在就创建                   |
| a+       | 读写，文件存在则指针放在末尾，文件不存在，创建新文件 |

* 使用**读写方式**打开，会影响文件读写效率，开发中更多的是**以只读，只写的方式**打开

### 按行读取
* `read()`方法默认读取全部，读取大文件对于系统内存占用会非常严重

#### readLine方法
* 一次读取一行，方法执行后，指针向下移动一行
* 读取大文件
```py
file = open("test")

while True:
    # 读取一行
    text = file.readLine()
    
    #判断是否读取到内容
    if not text:
        break
        
    #每读到一行末尾已经有"\n"
    print(text, end="")
    
# 关闭文件
file.close()
```

### 复制文件

```py
file_r = open("test")
file_w = open("test_copy")

while True:
    text = file_r.readLine()
    if not text:
        break
    file_w.write(text)

file_r.close()
file_w.close()
```

* 执行后打开`file_w`文件，发现已经复制了`file_r`的内容

## 文件/目录的常用管理操作

### 文件操作

| 方法   | 说明       | 示例                                |
|--------|------------|-------------------------------------|
| rename | 重命名文件 | os.rename("源文件名", "目标文件名") |
| remove | 删除文件   | os.remove("文件名")                 |

### 目录操作

```py
# 导入os模块
import os
```

| 方法       | 说明           | 示例                      |
|------------|----------------|---------------------------|
| listdir    | 目录列表       | os.listdir("目录名")      |
| mkdir      | 创建目录       | os.mkdir("目录名")        |
| rmdir      | 删除目录       | os.rmdir("目录名")        |
| getcwd     | 获取当前目录   | os.getcwd()               |
| chdir      | 修改工作目录   | os.chdir("目标目录")      |
| path.isdir | 判断是否为目录 | os.path.isdir("文件路径") |

### python编码
>* python2默认使用ascii编码，不支持中文
>* python3默认使用utf-8编码，支持中文

* 改变python2的编码格式，使支持中文，将下句代码写在文件开头
```py
# *-* coding:utf-8 *-*
```
或者
```py
# coding = utf-8
```

* 另外，使用python2输出中文时，会出现乱码，这是因为解释器会一个字节一个字节的输出，而中文一般是三个字节
```py
#!/usr/bin/python2
# *-* coding: utf-8 *-*

my_str = "hello, 你好"
print(my_str)

for letter in my_str:
    print(letter)
```

* 运行结果
```py

hello, 你好
h
e
l
l
o
,

�


�

```
> 输出乱码

* 解决方法：定义字符串时，前面加上字母`u`，说明这是utf-8格式的字符串
```py
my_str = u"hello, 你好"
```

* 再次运行
```py
hello, 你好
h
e
l
l
o
,

你
好
```

> 不再乱码

