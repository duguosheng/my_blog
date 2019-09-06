---
title: python--面向对象_多态
date: 2019-09-05 10:19:12
tags:
- python面向对象
categories:
- python
---

* 多态：不同的对象调用相同的方法，产生不同的执行结果，增加代码灵活度

<!--more-->

## 演示
```py
class Dog(object):
	def play(self):
		print("跑跑跳跳")
class Xtq(Dog):
	def play(self):     #重写父类方法
		print("飞到天上")
class Person(object):
	def play_with_dog(self, dog):
		print("快乐玩耍")
		dog.play()

wangcai = Dog() #普通狗旺财
xiaotian = Xtq() #哮天犬
xiaoming = Person()
xiaoming.play_with_dog(wangcai)	
print("*"*30)
xiaoming.play_with_dog(xiaotian)	#使用方法不变，传入另一个对象
```

* 执行结果
```py
快乐玩耍
跑跑跳跳
******************************
快乐玩耍
飞到天上
```


