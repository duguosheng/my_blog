---
title: python--面向对象_继承
date: 2019-09-05 09:11:11
tags:
- python面向对象
categories:
- python
---

* 继承：实现**代码的重用**，相同的代码不需要重复编写

<!--more-->

## 继承语法
```py
class 子类(父类):
    ...
```

* 继承的传递性：A继承自B，B继承自C，则A也拥有C的属性和方法
* 子类不能访问父类的私有属性或方法，但可以在父类方法中访问自身私有内容，子类调用该方法来间接访问

## 多继承
* 语法
```py
class 子类(父类1, 父类2):
    ...
```

* 注意：如果多继承中，不同的父类方法重名，则会优先使用继承顺序靠前的父类方法
```py
class A:
	def test(self):
		print("A--test")
	def demo(self):
		print("A--demo")

class B:
	def test(self):
		print("B--test")
	def demo(self):
		print("B--demo")

class C(A, B):
	pass

class D(B, A):
	pass

print("继承顺序是先A后B")
cc = C()
cc.test()
cc.demo()

print("继承顺序是先B后A")
dd = D()
dd.test()
dd.demo()
```

* 运行结果
```py
继承顺序是先A后B
A--test
A--demo
继承顺序是先B后A
B--test
B--demo
```

> 尽管不会报错，但不利于程序的阅读和理解，所以尽量不要使用多继承，使用时，也尽量保证属性方法不重名

### `__mro__` 
* 用于查看方法搜索顺序`类名.__mro__` 
* 如在上例中使用
```py
print(C.__mro__)
print(D.__mro__)
```

* 输出结果
```py
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
```


## 重写方法override
* 在子类中直接编写与父类中同名的方法即可
```py
class Animal:
	def voice(self):
		print("叫")
	def eat(self):
		print("吃")

class Cat(Animal):
	def voice(self):
		print("喵喵喵")

tom = Cat()
tom.eat()
tom.voice()
```

* 运行结果
```py
吃
喵喵喵
```

## 对父类方法进行扩展
* 调用父类方法
  1. `super().方法名`
  2. `父类名.方法(self)`  不推荐使用，因为当父类名改变时，语句也要改变
> 将上例中`Cat` 类中voice方法改成
```py
class Cat(Animal):
	def voice(self):
		super().voice() #第一种方法调用父类方法
        Animal.voice(self)  #第二种方法
		print("喵喵喵")
```
* 执行结果
```py
吃
叫
叫
喵喵喵
```


## 新式类和旧式类
* 新式类：以`object` 为基类的类，推荐使用
* 旧式类：不以`object` 为基类的类，不推荐使用
* python3中默认使用object类作为基类，即python3中定义的类全部是**新式类** 
* python2中如果没有指定父类，不会使用object类作为基类
> 新旧类会影响方法搜索顺序
保证python2与python3的统一，可以写作
```py
class 类名(object):
    ...
```


## 构造函数的继承
* 旧式类：父类名称.__init__(self,参数1，参数2，...)
* 新式类：super(子类，self).__init__(参数1，参数2，....)


```py
class A(object):
	def __init__(self, name):
		self.name = name

class B(A):
	def __init__(self, name, age):
        #调用父类初始化方法
		super(B, self).__init__(name)
		self.age = age

bb = B("das", 17)
print(bb.name)
```





