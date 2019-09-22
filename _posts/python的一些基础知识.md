---
title: python的一些基础知识
date: 2019-09-03 08:31:24
tags:
- python基础
categories:
- python
---

## python的运算符

<!--more-->

> 与C,java等不同

| 运算符                               | 描述     | 实例                                       |
| :---:                                | :---:    | ---                                        |
| +                                    | 加       | 10 + 20 = 30                               |
| -                                    | 减       | 10 - 20 = -10                              |
| *                                    | 乘       | 10 * 20 = 200                              |
| /                                    | 除       | 10 / 20 = 0.5                              |
| //                                   | 取整除   | 返回除法的整数部分（商） 9 // 2 输出结果 4 |
| %                                    | 取余数   | 返回除法的余数 9 % 2 = 1                   |
| **                                   | 幂       | 又称次方、乘方，2 ** 3 = 8                 |
| and                                  | 与       | a >= 1 and a <= 4                          |
| or                                   | 或       | a > 1 or a < 10                            |
| not                                  | 非       | not a < 1                                  |
| `+=` `-=` `*=` `/=` `%=` `//=` `**=` | 赋值运算 | a+=1等同于a=a+1                            |

## 查看关键字

```python
import keyword
print(keyword.kwlist)
```
---


## 语法

### if
```python
if a==1:
    print("aaa")
elif a==2:
    print("bbb")
else:
    print("ccc")
```

> `if`后面可以接**布尔运算，数字，字符串，列表，元组，字典等**，为真的条件是这些元素**为True，非0，非None**

* 条件分行
```python
# 使用()将条件括起来，在布尔运算符前可分行
if((a == 1 and b == 2)
        or (a ==2 and b ==3)):
    print("test")
```

### while
```python
while 条件:
    执行代码
```

### 定义函数
```python
# 定义的函数要在使用前声明
def 函数名(param1,param2):
    封装代码
    ...
    return res
```

### 注释
```python
# 单行注释

"""
这是
多行
注释
"""

# 在函数下方缩进后使用连续的三个'或“可以给函数编辑注释
# 如果此时直接按回车会生成带参数描述的文档注释
# 并且之后调用时可以使用Ctrl+Q快速查看
def test():
    '''this is a test example'''
    print("hello")
    
# TODO(作者/邮件) 这是TODO注释，提醒接下来要做的工作，可以在pycharm左下角找到所有TODO项
```

### print
* 数字和字符串输出
```python
age = 10
print("you are %d" % age)
```

* 多个数字和字符串输出
```python
num1 = 10
num2 = 20
num3 = 10.5
print("the number is %d , %d and %f" % (num1,num2,num3))    #实际上这是元组的应用
```
> %d是以十进制输出，可用%x以十六进制输出

* 使print不换行
```python
# print()方法默认是在尾部添加了换行符，而如果不想换行可以加,end=""
print("hello",end="")
print("world")

# 其他使用
print("hellp",end="***")
```


* 连续输出相同内容
```python
print("*" * 50)  #连续输出50个*
```


## 控制台输入
```python
input("请输入：")
```

控制台输入的内容是字符串，如果需要用于判断，则
```python
a = int(input("请输入"))
```
## 生成随机数
```python
import random
random.randint(10, 20) #生成10-20间的随机整数
```

## 导包
* 使用`import`关键字导入模块，所有以`.py`结尾的文件都可以看作一个模块
* 模块不能使用**数字**开头

### pyc文件
* 当使用import导入模块并编译后，会在编译文件夹中生成`.pyc`文件，这是使用`cpython`编译出的二进制文件，使得每次使用模块不需要反复解释，以提高执行性能

## 跳过
* 如果写了判断或其他情况暂时不写某些代码时，可以使用`pass` 充当占位符，使编译器不报错

```python
my_list = ["1", "2", "3"]
num = input("please input:")
if num in my_list:
    pass
else:
    pass
```

## shebang符号
* `#!` 是shebang符号，他后面加上解释器的路径即可在终端下通过输入文件地址而直接运行，且不影响在pycharm中运行
```shell
#! /usr/bin/python3
```

## id

* id(变量/数据)   查看其内存地址
