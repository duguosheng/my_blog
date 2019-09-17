---
title: '@property装饰器'
date: 2019-09-11 19:28:52
tags:
- property装饰器
- python基础
categories:
- python
description: property装饰器通常用来提供set和get方法
---

<!--more-->

### 功能
* `@property`装饰器可以让方法像属性一样进行调用

### 演示

#### 不使用@property装饰器
```py
class Person(object):
	def __init__(self, age):
		self.__age = age

	def set_age(self, age):
		self.__age = age

	def get_age(self):
		return self.__age

jack = Person(18)
print(jack.get_age())
jack.set_age(20)
print(jack.get_age())
```

* 运行结果
```py
18
20
```

#### 使用装饰器
```py
class Person:
	def __init__(self, age):
		self.__age = age

	@property
	def age(self):
		return self.__age
    
    # 注意这里是age.setter
	@age.setter
	def age(self, age):
		self.__age = age

jack = Person(18)
# 像属性一样调用方法
print(jack.age)
# 像属性一样调用方法
jack.age = 20
print(jack.age)
```

* 运行结果
```py
18
20
```



