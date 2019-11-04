---
title: 使用css样式的方式和基础选择器
date: 2019-10-10 20:27:42
tags:
- css基础
categories:
- css
description:
- css如何导入到网页中进行使用
---

<!--more-->

### 使用css的方式
#### 段内导入
```html
<p style="background-color:red;">hello world</p>
```

#### `head`中定义
```html
<style>
    p{
        background-color: gold;
    }
</style>
```

> 之后这个文件中所有的`<p>`都会为`gold`颜色

#### 导入其他的css文件
在`css`文件夹下新建`base.css`文件
在`<head>`中导入：

* 导入方式一
```html
<link rel="stylesheet" href="css/base.css" />
```

* 导入方式二
```html
<style>
    @import url("base.css");
</style>
```

### 基础选择器
#### 通用选择器
> 通用选择器是一个符号`*`，像通配符
```html
<style>
    *{
        background-color: red;
        margin: 0px;    /*取消所有的外间距*/
    }
</style>
```

#### id选择器
> id在文件中最好是唯一的
```html
<head>
    <style>
        /*使用#指定对应的id，多个id使用逗号隔开*/
        #d1, #p1{
            background-color: lightgreen;
        }
    </style>
</head>
<body>
    <div id="d1">你好，世界!</div>
    <br />
    <div id="p1">你好世界!</div>
</body>
```

#### 类选择器
```html
<head>
    <style>
        /*使用.指定类，多个类之间用逗号隔开*/
        .cls1, .cls2{
            background-color: purple;
            margin: 3px;
        }
    </style>
</head>
<body>
    <h1 class="cls1">你好</h1>
    <h2 class="cls1">你好</h1>
    <p class="cls2">你好</p>
    <br />
    <a href="www.baidu.com" class="cls2">百度</a>
</body>
```

> 另外一个标签可以属于多个类，多个类之间用`空格`隔开
```html
<h2 class="cls1 cls2">你好</h1>
```

#### 标签选择器
> 即指定对应的标签给定样式
```html
<style>
    /*给所有的input标签指定样式*/
    input{
        background-color: red;
    }
</style>
```

#### 选择器的优先级
* id>class>标签>通配符
