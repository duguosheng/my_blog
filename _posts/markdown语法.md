---
title: markdown语法
date: 2019-08-30 09:18:46
tags:
- 文本编辑
categories:
- markdown
---


# Markdown语法

<!--more-->

## 标题

```C
# 一级标题
## 二级标题
### 三级标题
```

*  n#+文字 n级标题

## 字体

```C
  **粗体**
  *斜体*
  ==高亮==
  ***斜体加粗***
  ~~删除线~~
  ^上标^
  ~下标~
```

## 列表

### 无序标题

* 使用"*"或者"+"后面跟空格，按一下回车会自动加标题，按两下会消失
> eg：

```C
- 一二三四五
- 上山打老虎
  - 下一级为空心圆
    - 再下一级是实心方块
      - 之后都是实心方块
```

效果
- 一二三四五
- 上山打老虎
  - 下一级为空心圆
    - 再下一级是实心方块
      - 之后都是实心方块



### 有序标题

与word类似
```C
1. 一级
2. 二级
```
效果
1. 一级
2. 二级


## 表格

第二行要有连字符

```C
| 姓名 | 学号 | 专业 | 班级 |
|------|------|------|------|
| 张三 | 037  | 机械 | 四班 |
```

效果

| 姓名   | 学号          | 专业 | 班级     |
| ------ | ------------- | ---- | -------- |
| 张三   | 037           | 机械 | 四班 |



## 引用

```C
> 大于号加引用内容
>> 二级引用，可多级
```

效果
> 大于号加引用内容
>> 二级引用，可多级


## 分割线
使用连字符，上一行不能有字符

```C
  上文

------

  下文
```

效果

  上文

------

  下文
  
## 代码

### 单行代码

使用``将代码括起来

```C
  `print("hello") `
```


### 多行代码
```java
  System.out.println("hello");
  System.out.println("hello");
  System.out.println("hello");
```

## 链接

```
1. <http://www.baidu.com>
2. [百度](http://www.baidu.com)
3. 我经常用[百度][1]搜索，很少用[必应][2]

[1]:http://www.baidu.com/ "Baidu"
[2]:http://www.bing.com/ "Bing"
```

效果

1. <http://www.baidu.com>
2. [百度](http://www.baidu.com)
3. 我经常用[百度][1]搜索，很少用[必应][2]

[1]:http://www.baidu.com/ "Baidu"
[2]:http://www.bing.com/ "Bing"

## 图片

```
![狮子](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=435466660,1319228756&fm=26&gp=0.jpg "this is a lion")
```

![狮子](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=435466660,1319228756&fm=26&gp=0.jpg "this is a lion")

## 百度地图

> 网址<http://api.map.baidu.com/lbsapi/creatmap/index.html>

注意要在html文件中加`<meta charset="UTF-8">`限定编码格式，否则**可能乱码**

```html5
<iframe src="~/Markdown/map.html" width="600" height="300" frameborder="0" scrolling="no"></iframe>
```

{% include_raw '_data/md_map.html' %}
