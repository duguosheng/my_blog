---
title: python中的异常
date: 2019-09-06 15:10:12
tags:
- python面向对象
categories:
- python
---

程序开发时很难将所有特殊情况都处理的面面俱到，通过异常捕获可以针对突发事件集中处理，从而保证程序的稳定性和健壮性

<!--more-->

## 捕获异常
### 简单捕获

* 格式
```py
try:
    尝试执行的代码
except:
    出现错误的处理
```

> 执行后会继续执行下方的代码

```py
try:
	num = int(input("请输入一个整数:  "))
except:
	print("请输入正确的整数")

print("继续执行了下方代码")
```
执行程序：用户输入了aaa，无法转化为整数
> 请输入一个整数:  aaa

* 执行结果
```py
请输入正确的整数
继续执行了下方代码
```

### 错误类型捕获
* 针对不同类型的异常，作出不同的相应
```py
try:
    #尝试执行的代码
    pass
except 错误类型1:
    #针对类型1处理
    pass
except (错误类型2, 错误类型3):
    #针对类型2和3处理
    pass
# 捕获未知错误，可以改变result的名字
except Exception as result:
    print("未知错误 %s" % result)
```

* 获取错误类型：出错时，控制台提示的最后一行就是错误类型，找到错误类型就可针对不同的错误进行不同的操作
```py
num = int(input("请输入一个整数:  "))
result = 10/num
print(result)
```

执行程序，用户输入0
> 请输入一个整数:  0

* 执行结果
```py
Traceback (most recent call last):
  File "/home/duguosheng/PycharmProjects/190902/d2_excpt.py", line 3, in <module>
    result = 10/num
ZeroDivisionError: division by zero
```

> 则**ZeroDivisionError**就是输入零时的错误类型，同理可得，输入字母时的错误类型是**ValueError**

* 针对不同错误类型的处理
```py
try:
	num = int(input("请输入一个整数:  "))
	result = 10 / num
	print(result)
except ZeroDivisionError:
	print("请不要输入0作为除数")
except ValueError:
	print("请输入数字")
```

* 捕获未知异常:假设未能预计到输入零的情况
```py
try:
	num = int(input("请输入一个整数:  "))
	result = 10 / num
	print(result)
except ValueError:
	print("请输入数字")
# 捕获未知异常
except Exception as result:
	print("未知错误 %s" % result)
```

用户输入0
> 请输入一个整数:  0

* 执行结果
```py
未知错误 division by zero
```

### 异常捕获完整语法
```py
try:
    pass
except 错误类型1:
    pass
except (错误类型2, 错误类型3):
    pass
except Exception as result:
    print(result)
else:
    #没有异常才会执行的代码
    pass
finally:
    #无论是否有异常，都会执行的代码
    pass
```

## 异常的传递
* 当函数/方法执行出现异常，会将异常传递给函数/方法的调用一方
* 如果**传递到主程序**，仍然**没有异常处理**，程序才会终止
> 开发中可以在主函数中增加异常捕获，这样就可以大大减少异常捕获代码，使代码整洁
```py
def test1():
    return 10/int(input("请输入一个整数：  "))
def test2():
	return test1()

#只在主函数中增加异常捕获
try:
	print(test2())
except Exception as result:
	print("未知错误 %s" % result)
```

输入0
> 请输入一个整数：  0

* 执行结果
```py
未知错误 division by zero
```

* 成功捕获异常并处理

## 主动抛出异常
> 当前函数只负责某项功能，主动抛出异常后，让其他函数再处理

* 主动抛出异常的方法
  * 创建一个`Exception`对象
  * 使用`raise`关键字抛出异常对象

* 实例
```py
def test():
	num = int(input("请输入一个整数：  "))
	if num > 0:
		return 10/num
        
    #主动抛出异常
	exception = Exception("请不要输入0")
	raise exception

#只在主函数中增加异常捕获
try:
	print(test())
except Exception as result:
	print(result)
```

输入0
> 请输入一个整数：  0

* 执行结果
```py
请不要输入0
```


