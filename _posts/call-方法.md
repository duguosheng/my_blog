---
title: __call__方法
date: 2019-09-17 09:06:28
tags:
- python面向对象
categories:
- python
description: 使对象可以像函数一样被调用
---

<!--more-->

* Python中，如果在创建类的时候写了`__call__()`方法， 那么该类实例化出实例后， `实例名()`就是调用`__call__()`方法。

```py
class SalaryAccount:
    """工资计算类"""
    def __call__(self, salary):
        print("发工资")
        year_salary = salary*12
        day_salary = salary//22.5
        hour_salary = day_salary//8

        return dict(year_salary=year_salary, month_salary=salary, day_salary=day_salary, hour_salary=hour_salary)

s = SalaryAccount()
print(s(3000))
```

> 执行结果
```py
发工资
{'year_salary': 36000, 'month_salary': 3000, 'day_salary': 133.0, 'hour_salary': 16.0}
```


