---
title: python函数
date: 2019-09-03 23:39:19
tags:
- python基础
categories:
- python
---

## 函数的返回值

<!--more-->

* 利用元组返回多个值
```python
def test():
    num = 1
    str = "hello"
    return (num, str)
    
temp = test()         #则temp是一个元组类型，对于不同的内容要使用对应索引调用，容易出错
temp1, temp2 = test() #或使用多个变量接收返回值，方便管理
```

* 同理，可利用元组交换数据
```python
a, b = (b, a)
a, b = b, a     # =右边是省略括号的元组
```

---


## 形参与实参
##### 在函数内部对变量使用赋值语句，会在函数内部修改局部变量的引用，不会影响到外部变量的引用
```py
def test(num, my_list):
    print("函数内部：")
    #赋值
    num = 10
    my_list = [4, 5, 6]
    print(num)
    print(my_list)
    
g_num = 5
g_list = [1, 2, 3]
test(g_num, g_list)
print("执行完成后")
print(g_num)
print(g_list)
```

* 执行结果
```py
函数内部：
10
[4, 5, 6]
执行完成后
5
[1, 2, 3]
```

> 实参未改变，其原因是变量与数据是分开存储的，`g_num` 指向数据`5` 的地址，当将`g_num`传入test函数是，实则是传入了数据`5` 的地址，而当使用赋值语句时，局部变量`num`指向新的数据`10` ，但`g_num`仍然指向`5` 

##### 如果传入的是可变类型，在函数内部使用方法修改了数据内容，会同时修改外部数据
```py
def test1(my_list):
	print("函数内部：")
	my_list.append(9)
	print(my_list)

g_list = [1, 2, 3]
test1(g_list)
print("程序结束：%s" % g_list)
```
* 输出结果
```py
函数内部：
[1, 2, 3, 9]
程序结束：[1, 2, 3, 9]
```
> 可见外部参数也被修改，其原因是使用数据的自身方法不会创建新的数据，而是在原有数据上的修改，外部变量和局部变量同时指向该地址，因而在函数内部使用方法修改后会影响外部参数


##### +=对于不可便类型是先相加后赋值，对于可变类型实质上是调用extend()方法
```py
def test2(num, my_list):
	num += 10
	print("不可变类型函数内部+=结果%d" % num)
	my_list += [8]
	print("可变类型函数内部+=结果%s" % my_list)


g_number = 5
g_l1 = [1, 2, 3]
test2(g_num,g_l1)
print("不可变类型外部结果%d" % g_number)
print("可变类型外部结果%s" % g_l1)
```

* 运行结果
```py
不可变类型函数内部+=结果15
可变类型函数内部+=结果[1, 2, 3, 8]
不可变类型外部结果5
可变类型外部结果[1, 2, 3, 8]
```

> 可见前者外部未改变，后者外部改变

---

## 给函数添加缺省值
* 在函数定义时可在参数后面加`=` 并添加缺省值
```py
def test3(name, gender=True):   #True是gender的缺省值
	"""
	输入姓名和性别
	:param name: 姓名
	:param gender: 性别 True男生 False女生
	:return: null
	"""
	gender_text = "男生"
	if not gender:
		gender_text = "女生"
	print(name+"是"+gender_text)

test3("小明")           #gender未输入，则使用缺省值
test3("小美", False)    #指定gender
```

* 输出结果
```py
小明是男生
小美是女生
```

* 注意：带有缺省值的参数必须放在参数列表的末尾

* 当有多个缺省参数而需要指定特定的某个参数值时，需要同时输入参数名
```py
def test4(name, gender=True, grade="大一"):
    #代码块
    ...

#如果指定gender
test4("小美", False)

#如果指定grade
test4("小明", grade="大二")
```

## 多值参数
* 有时一个函数传入的参数个数是不确定的，这个时候就可以使用**多值参数** 
* python中有两种多值参数
  * 参数名前加**一个** `*`可以接收**元组** 
  * 参数名前加**两个** `*` 可以接收`字典` 
* 一般给多值参数命名时，习惯使用以下名字
  * `*args` -- 存放元组  arguments(变量)
  * `**kwargs` --存放字典  kw是keyword
```py
def demo1(num, *args, **kwargs):
	print(num)
	print(args)
	print(kwargs)

demo1(1, 2, 3, 4, 5, name="小明", age=18)   #这种写法无须拆包
```

> 键不需要加分号，并用=连接

* 输出结果
```py
1
(2, 3, 4, 5)
{'name': '小明', 'age': 18}
```

## 元组和字典的拆包
* 在调用有多致参数的函数时，如果希望
  * 将元组直接传给`args` 
  * 将字典直接传给`kwargs`
* 就可以使用拆包
  * 在元组变量前加**一个**`*`
  * 在字典变量前加**两个** `*`


```py
def demo2(*args, **kwargs):
	print(args)
	print(kwargs)

g_nums = (1, 2, 3)
g_dict = {"姓名":"小明", "age":18}
demo2(g_nums, g_dict)
demo2(g_nums, **g_dict)
```

* 运行结果
```py
((1, 2, 3), {'姓名': '小明', 'age': 18})
{}
((1, 2, 3),)
{'姓名': '小明', 'age': 18}
```

> 可见前者字典也被传给了`args`，与期望不符，而后者加了`**` 实现预期

