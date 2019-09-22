---
title: python中关于全局变量
date: 2019-09-03 22:58:02
tags:
- python基础
categories:
- python
---

## python中不允许直接修改全局变量的值
<!--more-->
```python
num = 1     #定义全局变量

def test1():
    num = 2
    print(num)

def test2():
    print(num)

test1()
test2()
```

* 按照其他语言的逻辑，全局变量在test1()中被修改为2，则在test2()中也输出2，而最终的输出结果是

```python
2
1
```
> 并未如所想，其原因是python不允许直接修改全局变量的值，如果使用赋值语句，会在函数内部创建一个同名局部变量，而此局部变量在函数执行完成后就会被系统回收

## 全局变量的修改
* 使用`global` 声明变量即可

```python
num = 1     #定义全局变量

def test1():
    global num
    num = 2
    print(num)

def test2():
    print(num)

test1()
test2()

```

* 输出结果

```python
2
2
```

### 什么时候使用global
* 当对全局变量的指向进行修改的时候，需要使用`global`修饰，如果指向没有改变，则不需要，如：
```py
num = 10
num_list1 = [11, 22]
num_list2 = [11, 22]


def main():
    global num
    global num_list2
    num = 20  # 改变指向，需要声明
    num_list1.append(33)  # 不改变指向，不需声明
    num_list2 += [33, 44]  # 执行+=必须要声明global，因为这也是需要改变指向的操作，如果未声明程序就会崩溃


if __name__ == "__main__":
    main()
    print(num)
    print(num_list1)
    print(num_list2)
```

> 运行结果
```py
20
[11, 22, 33]  # 使用append()方法没有改变指向，成功达到预期执行结果
[11, 22, 33, 44]
```



## 全局变量的命名

* 可以在全局变量前加`g_` 或`gl_` 的前缀

