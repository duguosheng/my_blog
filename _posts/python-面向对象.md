---
title: python--面向对象_封装
date: 2019-09-04 20:19:25
tags:
- python面向对象
categories:
- python
---

* 封装：根据职责，将**方法** 和**属性** 封装到一个类中

<!--more-->

## dir()函数
* 使用dir()可以查看一个对象所有的可用方法
```py
def test():
	"""this is a example"""
    print("hello")
    
print(dir(test))    #查看可用方法
```

* 运行结果
```py
['__bool__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
```

* 使用这些方法
```py
print(test.__doc__)
```
* 运行结果
```py
this is a example
```

> 返回了函数注释信息

## 定义简单类-只包含方法

### 定义类
```py
class 类名:

    def 方法1(self, 参数列表)
        pass
    def 方法2(self, 参数列表)
        pass
```

* 类名使用**大驼峰命名法** 命名，首字母大写
* 类中的方法第一个参数必须是`self`，没有参数也要写`self` 
  
### 创建对象
```py
对象变量 = 类名()
```
> 先给创建出的对象开辟内存空间，再使`对象变量` 指向该地址

> 多次创建会开辟不同的内存空间


* 使用print()可以查看：对象的类和内存地址
```py
class Cat:
	def eat(self):
		print("i love fish")

tom = Cat()
print(tom)
```

* 输出结果
```py
<__main__.Cat object at 0x7f9c62f67290>
```

## 给对象增加属性
* 可以直接使用赋值语句，继续使用上个例子中创建的`tom`
```py
tom.name = "汤姆"   #这样tom就获得了一个name的属性
```

> 这种方式不推荐使用，应当把属性封装在类中

### 关于`self`
* 哪一个对象调用的方法，`self`就是哪一个对象的引用
```py
class Cat:
	def eat(self):
		print("%s 爱吃鱼" % self.name)

tom = Cat()
jf = Cat()
tom.name = "汤姆"
jf.name = "加菲"
tom.eat()
jf.eat()
```

* 输出结果
```py
汤姆 爱吃鱼
加菲 爱吃鱼
```

> 可见`self.name` 返回的是各自的属性

### 初始化方法
* 当使用类名创建对象时，自动执行
  1. 创建对象：在内存中分配空间
  2. 初始化：执行`__init__`方法，`__init__`是专门用来定义一个类具有那些属性的方法

* 自动执行`__init__`
```py
class Cat:
	def __init__(self):
		print("自动执行了__init__方法")

tom = Cat()
```
* 输出结果
```py
自动执行了__init__方法
```

### 在初始化内部定义属性
* 在`__init__` 内部使用`self.属性名 = 属性初始值` 就可以定义属性
* 定义属性后，使用类名创建的对象都将拥有该属性
```py
class Cat:
    def __init__(self):
        self.name = "tom"
```

* 这样创建的对象`name` 属性是相同的，若想创建的同时给对象指定不同的属性，可以给`__init` **加入参数** 
```py
class Cat:
    def __init__(self, name):
        self.name = name
        
tom = Cat("Tom")
```

## 内置方法和属性
### `__init__`
* 初始化时自动调用

### `__del__`
* 当对象被从内存中销毁前，会自动调用`__del__` 方法
```py
class Cat:
	def __init__(self, name):
		self.name = name
		print("%s被创建了" % name)
	def __del__(self):
		print("对象被销毁了")

tom = Cat("TOM")
print("*" * 30)
```

* 执行结果
```py
TOM被创建了
******************************
对象被销毁了
```

#### 使用`del` 关键字可以删除对象
```py
tom = Cat("TOM")
del tom
print("*" * 30)
```

* 执行结果
```py
TOM被创建了
对象被销毁了
******************************
```

> 可以使用`del` 关键字可以调用`__del__` 方法

### `__str__` 
* python中使用`print()` 输出对象，默认返回类和地址
* 可以使用`__str__` 自定义返回内容
* `__str` 必须返回一个字符串
```py
class Cat:
	def __init__(self, name):
		self.name = name
    def __str__(self):
		return "i am %s" % self.name

tom = Cat("TOM")
print(tom)
```

* 执行结果
```py
i am TOM
```

## 一个对象的属性可以是另一个类创建的对象
```py
class Cat:
	def __init__(self, name, mouse):
		self.name = name
		self.mouse = mouse
	def catch(self):
		print("%s抓住了%s" % (self.name, self.mouse.name))
        
class Mouse:
	def __init__(self, name):
		self.name = name

jerry = Mouse("Jerry")
tom = Cat("Tom", jerry)
tom.catch()
```

* 执行结果
```py
Tom抓住了Jerry
```

## `None` 关键字
* 当不知道给参数什么初值时，可以使用`None`，他表示一个没有方法和属性的空对象，是一个特殊的常量
* 可以给任意一个变量赋`None`
```py
a = None
self.name = None
```

## 身份运算符`is` 与`is not` 
* 身份运算符用于比较两个对象的**内存地址**是否一致--是否是对于同一个对象的引用
* 在python中使用`None` 比较时，建议使用`is` 和`is not` 
* `is` 与`==` 的区别
  * `is` 判断是否为同一个对象
  * `==` 判断值是否相等
```py
a = [1, 2, 3]
b = [1, 2, 3]
print(a == b)
print(a is b)
```

* 运行结果
```py
True
False
```

## 私有属性和私有方法
* 定义方法：在定义属性或方法时，在名字前增加**两个下划线**`__` 
* 私有属性：在对象的方法内部可以访问，在外界不能直接访问
```py
class Cat:
	def __init__(self, name, mouse):
		self.name = name
		self.__mouse = mouse

	def __str__(self):
		if self.__mouse:	# 在对象内部方法可以访问私有属性
			return "yes"
		else:
			return "no"


tom = Cat("Tom", True)
print(tom.name)
# print(tom.__mouse) 访问不到私有属性__mouse
print(tom)
```

* 执行结果
```py
Tom
yes
```

### 伪私有属性和方法
* 在python中，实际上没有完全的私有，对于私有属性和方法，在外部可以通过在属性或方法名前加`_类名`(下划线加类名)的方法访问，但尽量不要使用
```py
class Women:
	def __init__(self, name):
		self.name = name
		self.__age = 18
	def __secret(self):
		print("%s 年龄是%d" % (self.name, self.__age))

mary = Women("玛丽")
print(mary._Women__age)
mary._Women__secret()
```

* 执行结果
```py
18
玛丽 年龄是18
```




