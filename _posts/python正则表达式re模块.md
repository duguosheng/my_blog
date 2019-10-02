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
> `search`可以从文章的任意位置开始，但只能找到第一个满足条件的文本
```py
import re


result = re.search(r"\d+", "阅读次数为：4567; 点赞数为：345")
print(result.group())
```

> 运行结果
```
4567
```

> 如果使用`match`则`4567`必须在开头才能被匹配到
> 只能取到第一个满足的文本


## findall方法
* 可以检索到所有满足条件的文本
```py
import re


result = re.findall(r"\d+", "阅读次数为：4567; 点赞数为：345")
# 无需调用group，直接返回列表
print(result)
```

> 运行结果
```
['4567', '345']
```

## sub方法
* 可以更改数据
```py
re.sub(r"正则表达式", "要替换的文本", "被替换的文本")
```

* 示例
```py
result = re.sub(r"\d+", "123米", "长达4324, 宽为678")
print(result)
```

> 运行
```
长达123米, 宽为123米
```

> 全部都被替换了

### 嵌入表达式
* `sub()`还支持调用表达式
```py
import re


def add(temp):
    num = int(temp.group())
    num += 1
    return str(num)


new_content1 = re.sub(r"\d+", add, "长达4324, 宽为678")
print(new_content1)
```

> 运行结果
```
长达4325, 宽为679
```

> 实现了数据`+1`的操作


## spilt方法
* 根据匹配进行切割字符串，并返回一个列表
```py
import re


# 逗号，分号或空格分开字符串
result = re.split(r",|;| ", "abc def,gh;ij")
print(result)
```

> 运行结果
```
['abc', 'def', 'gh', 'ij']
```


