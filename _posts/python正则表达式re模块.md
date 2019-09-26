---
title: python正则表达式re模块
date: 2019-09-26 21:28:31
tags:
- 正则表达式
- re模块
categories:
- python
description: 介绍python中正则表达式的re模块
---

<!--more-->
## match方法

> `match()`方法默认判断开头
```
# 导入re模块
import re


# 使用match方法查看是否满足规则，如果匹配失败，返回None
# 字符串前加r表示这是非转义的原始字符串
ret = re.match(r"指环王[1-3]", "指环王2")
print(ret)

# 可以使用group方法查看匹配的对象
print(ret.group())
```

> 运行结果
```
<re.Match object; span=(0, 4), match='指环王2'>
指环王2
```

## search方法
> `search`可以从文章的任意位置开始
