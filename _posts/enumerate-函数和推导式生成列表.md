---
title: enumerate()函数和推导式生成列表
date: 2019-09-17 16:29:04
tags:
- python文件操作
categories:
- python
description: 操作增加行号
---

<!--more-->
## 描述
* `enumerate()` 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。

## 语法
* 以下是 enumerate() 方法的语法:
```py
enumerate(sequence, [start=0])
```

> 参数
>> sequence -- 一个序列、迭代器或其他支持迭代对象。
>> start -- 下标起始位置。
> 返回值
>> 返回 enumerate(枚举) 对象。

### 示例
```py
letters = ["a", "b", "c", "d"]
gnt = enumerate(letters, start=1)
print(gnt)
# 需要调用list方法
my_enum = list(gnt)
print(my_enum)
```

> 结果
```py
<enumerate object at 0x7f67e867f2d0>
[(1, 'a'), (2, 'b'), (3, 'c'), (4, 'd')]
```

## 实现给文件的每行增加行号
* 新建文本文件`test.txt`，里面写上
```
窗前明月光
疑是地上霜
举头望明月
低头思故乡
```

* 在python文件写入以下代码
```py
with open(r"test.txt", "r", encoding="utf-8") as my_file:
    lines = my_file.readlines()
    lines = ["#"+str(index+1)+" "+line for index, line in list(enumerate(lines))]  # 推导式生成推导式
    # 也可写成lines = ["#"+str(index+1)+" "+line for index, line in enumerate(lines)]

with open(r"test.txt", "w", encoding="utf-8") as my_file:
    my_file.writelines(lines)
```

> 打开`test.txt`查看执行结果
```
#1 窗前明月光
#2 疑是地上霜
#3 举头望明月
#4 低头思故乡
```
