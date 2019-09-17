---
title: close()关闭流
date: 2019-09-17 15:57:22
tags:
- python文件操作
categories:
- python
description: 在进行文件的操作后需要使用`close()`方法来关闭文件，释放内存
---

<!--more-->
* 为了确保打开的文件对象正常关闭，一般结合异常机制的`finally`或者`with`关键字实现无论何种情况都能关闭打开的文件对象


## 使用finally
```py
try:
    f = open(r"my_text.txt", "a")
    my_str = "hello"
    f.write(str)
except BaseException as e:
    print(e)
finally:
    f.close()
```

## 使用with
* `with`关键字(上下文管理器)可以自动管理上下文资源，不论什么原因跳出`with`块，都能确保文件正常的关闭，并且可以在代码块执行完毕后自动还原进入该代码块时的现场
```py
with open(r"test.txt", "a") as my_file:
    my_file.write("hello")
```

> 执行完后，可以自动关闭文件

