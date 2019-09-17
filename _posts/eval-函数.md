---
title: eval()函数
date: 2019-09-10 15:45:25
tags:
  - eval
  - python进阶
categories:
  - python
description: eval()可以将字符串当成有效的表达式来求值并返回计算结果
---

<!--more-->

## 功能描述
```py
# 数学计算
input:  eval("1+1")
output: 2

# 字符串重复
input:  eval("'*' * 10")
output: **********

# 将字符串转化为列表
input:  type(eval("[1, 2, 3, 4]"))
output: list

# 将字符串转换为字典
input:  type(eval("{'name': 'xiaoming', 'age': 18}"))
output: dict
```

### 演示案例：计算器
```py
print(eval(input("请输入计算式: ")))
```

* 执行
```py
请输入计算式: 5+2*5**2
55
```

### 不要滥用eval
* 使用eval()直接转换输入结果可能导致安全漏洞，如上例中用户输入`__import__('os').system('ls')`
* 等价于
```py
import os
os.system("ls")
```

* 运行
```py
请输入计算式: __import__('os').system('ls')
test1.py
test2.py
0
```

> 输出了当前目录下文件列表，同理也可执行其他增删改的终端命令，很不安全

* 执行成功，返回0
* 执行失败，返回错误信息
