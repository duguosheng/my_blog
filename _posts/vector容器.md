---
title: vector容器
date: 2019-11-09 19:46:52
tags:
- 容器
categories:
- C/C++
description:
- vector容器是一个能够存放任意类型的动态数组
---

转载自[码农有道](https://blog.csdn.net/wucz122140729/article/details/98582170)
<!--more-->

之前我们在声明数组的时候，采用的是datatype  array[len]的形式，数组在分配之后，不能调整大小，删除和插入数据时操作十分的繁琐，虽然可以采用链表，但是链表的操作更麻烦，我喜欢简单的方法。

与string类一样, 向量vector 同属于STL（Standard Template Library, 标准模板库）中的自定义的类, vector是一个封装了动态数组的顺序容器（Sequence Container）。跟其它类型的容器一样，它能够存放各种类型的数据和对象。可以简单的认为，vector容器是一个能够存放任意类型的动态数组。

与数组相比，vector 容器的优点在于它能够根据需要自动调整的大小，随时放入更多的元素。此外, vector 也提供了许多的成员函数来对自身进行操作。

### 容器的定义
首先，如果要在程序中使用vector容器，必须包含头文件`<vector>`。如下：
```C++
#include <vector>
```

vector类是一个模板类，位于std命名空间内，为方便使用还需要增加：
```C++
using namespace std;
```

声明一个容器很简单：
```C++
vector<int> vi;              // 定义用于存放整数的容器
vector<double> vd;         // 定义用于存放浮点数的容器
vector<string> vs;           // 定义用于存放string字符串的容器
vector<struct st_girl> vgirl;  // 定义用于存放超女结构体的容器
vector<CGirl> vGirl;         // 定义用于存放超女类的容器
```

vector容器可以存放C语言的基本数据类型，可以存放结构体，还可以存放类，这正是我们想要的简单的方法，至于链表，我已经有二十年没有用它了。

### 容器的使用
vector的功能强大，成员函数很多，我不想按普通教程的方式来介绍它，那样会太烦锁，我回忆了二十年来的实际应用场景，采用示例程序介绍vector常用的用法。

#### 存放整数
* 示例
```C++
#include <stdio.h>
#include <vector>       //vector需要的头文件
#include <algorithm>    //sort需要的头文件
using namespace std;

int main()
{
    int height = 0;
    vector<int> vheight;

    while(true)
    {
        printf("请输入超女的身高(0-结束输入)：");
        scanf("%d", &height);
        if(height==0)
            break;
        vheight.push_back(height);  //把数据追加入容器
    }

    for(int i=0; i<vheight.size(); i++)
    {
        printf("vheight[%d]=%d\n", i, vheight[i]);
    }

    sort(vheight.begin(), vheight.end());  //容器中的记录排序

    for(int i=0; i<vheight.size(); i++)
    {
        printf("vheight[%d]=%d\n", i, vheight[i]);
    }

    vheight.clear();  //清空容器
}
```

> 运行结果
```
请输入超女的身高(0-结束输入)：166
请输入超女的身高(0-结束输入)：165
请输入超女的身高(0-结束输入)：170
请输入超女的身高(0-结束输入)：0
vheight[0]=166
vheight[1]=165
vheight[2]=170
vheight[0]=165
vheight[1]=166
vheight[2]=170
```

* 程序解释如下：
`std::vector<int> vheight;`   声明容器用于存放整数，超女身高，单位是cm。

`void push_back(const T& x)`成员函数：向容器尾部增加一个元素x，x就是你要向容器中增加的变量，在本示例中是一个整数，注意，参数x是一个引用，要用变量的名称，不是变量的地址，如vheight.push_back(height);。

`int size()`成员函数：返回容器中数据的元素总数。

像数组一样访问容器：容器像数组一样，可以用数组的下标访问，vheight[0]表示容器的第1个元素，vheight[n]表示容器的第n+1个元素。注意，采用下标方式放问容器的时候，下标不要越界，否则可能会引起内存溢出。

`iterator begin()`成员函数：返回容器头指针，指向第一个元素。

`iterator end()`成员函数：返回容器尾指针，指向容器最后一个元素的下一个位置。

`sort()`容器中的元素排序：采用sort()函数对容器中的元素进行排序。如sort(vheight.begin(),vheight.end());表示对容器中全部的元素进行排序。

`void clear()`：清空容器中的全部元素，注意两点：1）容器被声明的时候，本来就是空的；2）容器是类，有析构函数，析构函数中会自动清空容器中的元素，释放内存资源，不需要程序员担心。但是，程序员也可以调clear()函数手工清空容器中的元素。

示例程序展示了vector容器10%的功能和成员函数，但是，在实际开发中，对容器的操作，95%的内容就是这些。

#### 存放字符串
示例
```C++
#include <stdio.h>
#include <vector>
#include <string>
#include <string.h>
#include <algorithm>
using namespace std;

int main()
{
    char strtmp[50];  //存放姓名的临时变量
    string name;  //存放键盘输入的名字
    vector<string> vname;  //存放姓名的容器

    while(true)
    {
        printf("请输入姓名(0-结束输入):");
        scanf("%s", strtmp);  //接收键盘输入
        if(strcmp(strtmp, "0")==0)
            break;  //结束输入
        vname.push_back(strtmp);  //把数据加入容器
    }

    for(int i=0; i<vname.size(); i++)
    {
        printf("vname[%d]=%s\n", i, vname[i].c_str());
    }
}
```

> 运行结果
```
请输入姓名(0-结束输入):张三
请输入姓名(0-结束输入):李四
请输入姓名(0-结束输入):王五
请输入姓名(0-结束输入):赵六
请输入姓名(0-结束输入):0
vname[0]=张三
vname[1]=李四
vname[2]=王五
vname[3]=赵六
```

注意几个问题：

1）用容器存放字符串，数据类型用string，不是C语言用0结尾的字符数组char[]。

2）用vname.push_back()成员函数把数据追加到容器中，参数的类型可以是string，也可以是char[]，但是，这并不是vector的特征，而是string的特征，容器声明的是string类型，string的构造函数支持char []，表面上看push_back()进去的是char []，实际上会被转换为string。

3）对string和结构体等的排序，在以后的章节中会详细介绍。

#### 存放结构体
* 示例
```C++
#include <stdio.h>
#include <string.h>
#include <vector>
using namespace std;

typedef struct{
    char name[30];  //姓名
    int age;        //年龄
}PERSON;

int main()
{
    PERSON per;
    vector<PERSON> vper;

    strcpy(per.name, "张三");
    per.age = 19;
    vper.push_back(per);

    strcpy(per.name, "李四");
    per.age = 22;
    vper.push_back(per);

    //采用数组下标访问容器中的记录
    for(int i=0; i<vper.size(); i++)
    {
        printf("vper[%d].name=%s;vper[%d].age=%d\n",\
                i, vper[i].name, i, vper[i].age);
    }

    memset(&per, 0, sizeof(per));//清空结构体
    for(int i=0; i<vper.size(); i++)
    {
        printf("per%d.name=%s;per%d.age=%d\n",\
                i, per.name, i, per.age);
    }

    for(int i=0; i<vper.size(); i++)
    {
        memcpy(&per, &vper[i], sizeof(per));
        printf("per%d.name=%s;per%d.age=%d\n",\
                i, per.name, i, per.age);
    }
}
```

> 运行结果
```
vper[0].name=张三;vper[0].age=19
vper[1].name=李四;vper[1].age=22
per0.name=;per0.age=0
per1.name=;per1.age=0
per0.name=张三;per0.age=19
per1.name=李四;per1.age=22
```

总的来说，存放结构体的容器和数组的用法基本相同。

在book225.cpp中，采用了memcpy函数，它是C语言的库函数，用于内存中的数据复制，声明如下：
```C++
void *memcpy(void *dest, const void *src, size_t n);
```

dest -- 指向用于存储复制内容的目标地址，类型强制转换为`void*` 指针。
src -- 指向要复制的数据源地址，类型强制转换为`void*` 指针。
n -- 要被复制的字节数。

该函数返回dest。

#### 存放类
在第2节存放字符串中，string就是类。

vector容器可以存放类，但是在实际开发中，除了存放string类，很少用vector存放其它的类。

### 其它成员函数
vector的成员函数比较多，为了不增加各位的学习负担，我介绍一些可能有用的。

#### 定位的函数
iterator begin()：返回容器头的指针，指向容器第一个元素的位置。
iterator end()：返回容器尾的指针，指向容器最后一个元素的下一个位置。
iterator是跌代器，这个名字让人难以理解，大家不要管它，以下我会举例说明用法，一般来说，begin()和end()成员函数会用在其它成员函数的参数中。

#### 增加元素的函数
void push_back(const T& x)：向容器的尾部增加一个元素x。
iterator insert(iterator it,const T& x)：向容器中指定位置（it）前插入一个元素x。

示例：
strcpy(stgirl.name,"王昭群"); stgirl.age=22;
vgirl.insert(vgirl.begin()+1,stgirl);  // 在第2个元素前插入一个元素。

#### 删除元素的函数
iterator erase(iterator it)：删除容器中指定位置（it）的元素。

示例：
vgirl.erase(vgirl.begin()+2);  // 删除容器中第3个元素。
void pop_back()：删除容器中最后一个元素。
void clear()：清空容器中全部的元素。

#### 判断容器的大小
bool empty()：判断容器是否为空。
int size()：返回容器中元素的个数。
