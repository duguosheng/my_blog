---
title: python的高级数据类型
date: 2019-09-03 12:22:23
tags:
- python基础
categories:
- python
---

## 获取元素类型

<!--more-->

```python
# 使用type()获取数据类型
my_list = [1, 2, 3]
print(type(my_list))
```

## 列表
* 类似于其他数据类型中的**数组**
* 列表使用`[]`括起来
* 列表中可以存储不同类型的数据类型

```python
my_list = ["aaa","bbb","ccc"]
print(my_list[0])   #取值
print(my_list.index("aaa")) #取索引

my_list[0] = "ddd"  #修改

my_list.append("eee")   #增加数据
my_list.insert(1,"fff") #插入数据在my_list[1]，后面的数据后移
temp_list = ["a","b"]
my_list.extend(temp_list)   #将另一个列表扩展进来

my_list.remove("bbb")   # 删除指定元素,如果出现多次，删除第一个
my_list.pop()       #默认删除最后一个元素（出栈）
my_list.pop(3)      #删除my_list[3]
my_list.clear       #清空列表

del my_list[1]      #利用del删除（直接从内存中删除，后续代码不可使用该变量）
a = 10
del a

length = len(my_list)   #读取列表大小
time = my_list.count("aaa")     #读取aaa出现的次数

my_list.sort()      #升序排序
my_list.sort(reverse=True)  #降序排序
my_list.reverse()   #逆序反转
```

* 迭代遍历
```python
for temp_name in my_list:
    print("the name is %s" % temp_name)
```

### 创建列表的方式
* 基本语法
```py
my_list = [1, 2, "hello"]
```

* `list()`创建
```py
my_list1 = list("abcde")
my_list2 = list(range(1, 10, 2))
print(my_list1)
print(my_list2)
```

> 结果
```
['a', 'b', 'c', 'd', 'e']
[1, 3, 5, 7, 9]
```

> 利用`range()`可以快速创建整数列表，其格式为`range([start,] end [,step])`，它返回的是一个range对象
>> `start`：起始数字，默认0
>> `end`：结尾数字，必须指定
>> `step`：步长，默认1

* 推导式生成列表
```py
my_list3 = [x*2 for x in range(5)]
my_list4 = [x**2 for x in range(50) if x%5==0]
my_list5 = [(x, y) for x in range(5) for y in range(5) if x%2==0 if y%3==0]
my_list6 = [a for a in "hello"]
print(my_list3)
print(my_list4)
print(my_list5)
print(my_list6)
```

> 结果
```py
[0, 2, 4, 6, 8]
[0, 25, 100, 225, 400, 625, 900, 1225, 1600, 2025]
[(0, 0), (0, 3), (2, 0), (2, 3), (4, 0), (4, 3)]
['h', 'e', 'l', 'l', 'o']
```




## 元组
* 元组与列表相似，不同之处在于元组的元素**不能修改** 
* 使用`()` 定义
```python
# 定义空元组
empty_tuple = ()

# 定义单元素元组
single_tuple = (5)   #这样定义得到的single_tuple是int类型
single_tuple = (5,)  #正确

my_tuple(1)     # 取值
my_tuple.index("aaa")   #取索引
my_tuple.count("aaa")   #统计aaa出现次数
len(my_tuple)       #统计长度
```

* 遍历元组
```python
for temp_info in my_tuple:
    执行代码
    ...
```

### 元组的应用

* 拼接字符串
```python
my_tuple = ("zhangsan", 18, 175.5)
print("%s的年龄是%d,身高是%f" % my_tuple)

my_str = "%s的年龄是%d,身高是%f" % my_tuple
print(my_str)
```

* 保护列表安全
```python
my_list = [1, 2, 3, 4]
my_tuple1 = tuple(my_list)      #使用tuple()将list转换成tuple
my_list2 = list(my_tuple1)      #使用list()将tuple转换成list
```

* 利用元组返回多个值(同理也可使用列表，字典，集合)
```python
def test():
    num = 1
    str = "hello"
    return (num, str)
    
temp = test()       #则temp是一个元组类型
temp1, temp2 = test() #多个变量接收返回值
```

* 交换数字
```python
a, b = (b, a)
```

### 生成器推导式生成元组
```py
my_tp1 = (x*2 for x in range(5))
my_tp2 = (x**2 for x in range(50) if x%5==0)
# 打印生成器
print(my_tp1)
print(my_tp2)
# 打印生成的元组，注意只能调用一次tuple()
print(tuple(my_tp1))
print(tuple(my_tp2))
```

> 结果
```
<generator object <genexpr> at 0x7fb5bd8cc850>
<generator object <genexpr> at 0x7fb5bd8cc8d0>
(0, 2, 4, 6, 8)
(0, 25, 100, 225, 400, 625, 900, 1225, 1600, 2025)
```


## 字典
* 使用`{}`定义
* 列表是有序的数据集合
* 字典是无序的数据集合
* 使用`键值对`存储数据
  * **键** *(key)* 是索引，必须**唯一** ，只能是**字符串，数字，或者元组** 
  * **值** *(value)* 是数据，可以是任意数据类型
  * *key* 和*value* 之间使用`:` 分隔

```python
# 由于字典无序，通常使用print()打印的结果与定义的顺序不同

#定义
xiaoming = {"name": "小明", 
            "age": 18, 
            "height": 175}
            
xiaoming["name"]        #取值
xiaoming["weight"] = 70 #不存在则增加
xiaoming["age"] = 19    #若存在则修改

xiaoming.pop("weight")  #删除

len(xiaoming)           #统计键值对数目

temp_dict = {"gender": boy, 
             "age": 20}
xiaoming.update(temp_dict)  #若新增key不存在，则增加;若key重复，则会替换掉之前的value

xiaoming.clear()        #清空
xiaoming.
```

* 遍历字典
```python
my_dict = {"name": "zhangsan", 
           "age": "18"}
           
# 变量temp是my_dict中的key
for temp in my_dict:
    print("%s - %s" % (temp,my_dict[temp]))
```
* 应用
先用字典存储复杂数据，再将多个字典放在一个列表中管理

```python
card_list = [
    {"name": "zhangsan", 
     "qq": "12345"}
    {"name": "lisi", 
     "qq": "12412"}
]

for info in card_list:
    print(info)
```

### 创建字典的几种方式
* 使用{}基本语法
```py
my_dict1 = {"name":"张三", "age":28}
```

* 使用`dict()`方法
```py
my_dict2 = dict(name="张三", age=18)
my_dict3 = dict(("name", "张三"), ("age", 18))
```

* 使用`zip()`
```py
my_key = ["name", "age", "job"]
my_value = ["张三", 18, "teacher"]
my_dict4 = dict(zip(my_key, my_value))
print(my_dict4)
```

> 运行结果
```
{'name': '张三', 'age': 18, 'job': 'teacher'}
```

* 通过`fromkeys`创建值为空的字典
```py
my_dict5 = dict.fromkeys(["name", "age", "job"])
print(my_dict5)
```

> 结果
```
{'name': None, 'age': None, 'job': None}
```

* 字典推导式
```py
# 利用推导式统计字符出现次数
my_text = "hello, world"
my_dict6 = {my_char:my_text.count(my_char) for my_char in my_text}
print(my_dict6)
```

> 结果
```py
{'h': 1, 'e': 1, 'l': 3, 'o': 2, ',': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1}
```

## 集合
> 集合无序可变，**元素不能重复**，集合底层是字典实现，集合中的元素都是字典中的`键对象`，因此不可重复

* 使用{}创建集合对象，并使用`add()`添加元素
```py
a = {"name", "age", 3, 5}
a.add(9)
```

* 使用`set()`可以将列表，元组等可迭代对象转化为集合，如果数据重复，只保存一个
```py
a = ["a", "b", "c", "a"]
b = set(a)
print(b)
```

> 结果
```py
{'a', 'c', 'b'}
```

* `remove()`删除指定元素，`clear()`清空集合
```py
a.remove("a")
a.clear()
```

* 集合相关操作
```py
a = {1, 2, 3}
b = {2, "a", "c"}
# 并集
print(a|b)
print(a.union(b))
# 交集
print(a&b)
print(a.intersection(b))
# 差集
print(a-b)
print(a.difference(b))
```

> 结果
```py
{1, 2, 3, 'a', 'c'}
{1, 2, 3, 'a', 'c'}
{2}
{2}
{1, 3}
{1, 3}
```



## 字符串
```python
# 字符串定义：使用''或""括起来的内容
str1 = "hello"
str2 = 'world'  #为了和其他语言统一，尽量不使用单引号

#当需要字符串中包含引号时
str3 = "my name is "dgs""   #错误
str4 = 'my name is "dgs"'   #正确

#取值
str = "abcdefabcbac"
str[2]  #第三个字符
str[-1] #倒数第一个字符

#迭代遍历
for temp in str:
    print(temp)

len(str)        #统计长度
str.index(”e“)    #获取索引
str.count(”a“)    #统计a出现的次数

# 字符串切片  字符串[开始索引:结束索引:步长]，不包含结尾处索引的内容
str[2:6]    #截取第二个到第六个
str[2:]     #从第二个到末尾
str[2:-1]   #从第二个到倒数第一个
str[:6]     #从开头到第六个
str[::2]    #每隔一个截取一个
str[-1::-1] #获得字符串倒序
```

## 公共方法

| 函数              | 描述                                      | 备注                                        |
|-------------------|-------------------------------------------|---------------------------------------------|
| len(item)         | 计算容器中元素个数                        |                                             |
| del(item)         | 删除变量                                  | `del` 有两种方式`del(temp[1])` /`del(temp)` |
| max(item)         | 返回容器元素最大值                        | 字典只比较key                               |
| min(item)         | 返回容器元素最小值                        | 字典只比较key                               |
| cmp(item1, item2) | 比较两个值，`-1` 小于，`0` 等于，`1` 大于 | python3取消了cmp                            |

* 字符串比较遵循：`0` <`A` <`a` 
  
## 切片

```python
l1 = [1, 2, 3, 4, 5][::2]
t1 = ("s", 2, "hello", 4, 5)[::2]
# 字典不能切片
```

## 运算符

| 运算符     | 演示                | 结果             | 描述                    | 支持类型                 |
|------------|---------------------|------------------|-------------------------|--------------------------|
| +          | [1, 2]+[3, 4]       | [1, 2, 3, 4]     | 合并                    | 字符串，列表，元组       |
| *          | ("hi")*3            | ("hi","hi","hi") | 重复                    | 同上                     |
| in         | 3 in (1, 2, 3)      | True             | 是否存在，字典判断key   | 字符串，列表，元组，字典 |
| not in     | 4 not in (1, 2, 3)  | True             | 是否不存在，字典判断key | 同上                     |
| `<` `>` 等 | (1, 2, 3)<(2, 3, 4) | True             | 比较                    | 字符串，列表，元组       |

> `+` 与`entend()` 方法的区别：前者生成一个新的变量，后者追加到调用该方法的变量

* append()与extend()区别
```python
l2 = [1, 2, 3]
l2.append(4)
print(l2)
# 不能l2.extend(5)，extend()只能传入容器
l2.append([6, 7])
print(l2)
l2.extend([8, 9])
print(l2)
```
* 输出结果
```python
[1, 2, 3, 4]
[1, 2, 3, 4, [6, 7]]        # 将append([6, 7])当成了一个元素
[1, 2, 3, 4, [6, 7], 8, 9]  # 将extend([8, 9])追加
```

## 完整的for循环
```python
for 变量 in 集合:
    循环体代码
else:
    没有通过break退出循环，遍历结束后就会执行
```

* 应用
遍历完成后，如果没有查询的就提示

## 其他
* 对变量使用自身的方法进行增删改不会改变id，而重新赋值会改变，这时它的指针指向了另一个地址
