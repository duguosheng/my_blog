---
title: python的模块和包
date: 2019-09-06 16:36:05
tags:
- python进阶
categories:
- python
---

# 模块
每一个以`py`结尾的python源代码文件都是一个模块
<!--more-->

## 模块导入
### import导入
#### 格式

* 不推荐格式：将几个模块放在一行
```py
import 模块名1, 模块名2
```
* 推荐:每个模块单独占一行
```py
import 模块名1
import 模块名2
```

* 导入之后可以通过`模块名.`的方式使用模块提供的工具：**全局变量，类，函数**

#### 使用as指定模块别名
* 如果模块名字太长就可以使用`as`来指定别名，方便实用
```py
import 模块名 as 模块别名
```

> 模块别名应该符合**大驼峰命名法**

### from...import导入
* 如果希望从某一个模块中，导入**部分工具**，就可以使用这种方式
* 语法格式
```py
# 从模块导入某一个工具
from 模块名 import 工具名
```
> 导入之后不需要通过`模块名.`就可以直接使用模块的工具
> 如果两个模块存在**同名的函数**，那么后导入模块的函数，会**覆盖掉先导入的函数**

* 可以通过`as`别名的方式调用前面导入的函数
```py
from pkg1 import test as pkg1_test  #给test()函数起别名
from pkg2 import test   # pkg1和pkg2中同时含有test()方法

pkg1_test()     #调用pkg1的test()
test()      #调用pkg2的test()
```

#### 导入全部
* 语法
```py
# 从 模块 导入 所有工具
from 模块名 import *
```

> 不推荐使用，因为函数重名不好排查

## python导入模块的顺序
1. 搜索**当前目录**指定模块名的文件，如果有就直接导入
2. 如果没有，再搜索系统目录
> 在开发时创建文件，命名不要和系统模块文件重名，否则调用系统方法时会因为当前目录下存在该模块而不去搜索系统目录，使程序无法正确执行
* 使用内置方法`__file__`可查看文件完整路径
```py
print(模块名.__file__)
```

## 关于模块导入
* 在导入模块时，文件中没有缩进的代码**都会被执行一遍**
```py
# 这是test1模块中的内容
print("这是模块一")
```

```py
# 这是test2模块中的内容
import test1
print("这是模块二")
```

* 在模块test2中`run`，执行结果
```py
这是模块一
这是模块二
```

而有时开发中开发者要做一些测试，写的一些代码只希望在本文件内执行而不想在被导入时执行

### `__name__`属性
> `__name__`属性可以做到，测试模块的代码只在测试时执行，在被导入时不执行

* `__name__`属性是python的一个内置属性，记录着一个字符串
* 如果是被其他文件导入的，则记录的是**模块名**
* 如果是当前执行的程序，`__name__`是`__main__`
```py
# 模块test1中
print(__name__)
```

```py
# 模块test2中
import test1
print(__name__)
```

* 在test2中`run`，运行结果
```py
test1
__main__
```

* 所以在测试时可以通过`__name__`来实现代码仅在测试时执行
```py
# 模块test1中
if __name__ == "__main__":
    print("这是模块一")
```

```py
# 模块test2中
import test1
print("这是模块二")
```

* 在模块test2中`run`，运行结果
```py
这是模块二
```

* 编写代码格式
```py
...
...
...

# 文件末尾，编写本地测试代码
def main():
    ...
    pass
    
if __name__ == "__main__":
    main()
```


# 包
* **包**是一个包含多个模块的**特殊目录**
* 目录下有一个特殊的文件**`__init__.py`**
* 包的命名和变量一致，使用`小写字母`和`_`，如`this_is_a_pkg`
* 可以使用`import 包名`的方式，一次性导入包中所有的模块
* 在pycharm中鼠标停在工程名上点击鼠标右键
  * 选择new --> Python Package可以建立包并自动创建`__init__.py`空文件
  * 或选择new --> Directory创建文件目录，再自行创建`__init__.py`文件
* `__init__.py`文件
  * 包中可以对外界使用的模块，需要在`__init__.py`中写出列表
```py
# 从 当前目录 导入 模块列表
from . import 模块名称1
from . import 模块名称2
```

# 发布模块包的步骤

## 发布包

* 创建`setup.py`文件
```py
from distutils.core import setup

setup(name="pkg_name",  # 包名
      version="1.0",  # 版本
      description="some text",  # 描述信息
      long_description="完整的发送和接收消息模块",  # 完整描述信息
      author="itheima",  # 作者
      author_email="abc.com",  # 作者邮箱
      url="www.abc.com",  # 主页
      py_modules=["包名.模块名1",  #要分享的模块
                  "包名.模块名2"])
```

有关字典参数的详细信息，可以参阅[官方网站](https://docs.python.org/2/distutils/apiref.html) 

* 构建模块
```bash
python3 setup.py build
```

* 生成发布压缩包
```bash
python3 setup.py sdist
```
> 注意：要制作哪个版本的模块，就使用哪个版本的解释器执行！


## 下载包

* 安装模块
```bash
tar -zxvf 压缩包名.tar.gz 
sudo python3 setup.py install
```

* 卸载模块
直接从安装目录下，把安装模块的目录删除就可以
```bash
cd /usr/local/lib/python3.7/dist-packages/
sudo rm -r 包名*
```

* 可以使用pip安装第三方模块
```bash
# 将第三方模块安装到python2.x环境
sudo pip install 模块
sudo pip uninstall 模块

# 将第三方模块安装到python3.x环境
sudo pip3 install 模块
sudo pip3 uninstall 模块
```



