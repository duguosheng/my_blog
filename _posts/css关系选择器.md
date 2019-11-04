---
title: css关系选择器和属性选择器
date: 2019-10-11 20:28:34
tags:
- css关系选择器
categories:
- css
description:
- css关系选择器
---

<!--more-->

### 关系选择器

* 包含关系选择器
```html
element1 element2
```

> 选取所有包含于`element1`中的`element2`元素，可以是子级，孙级等等
```html
<head>
    <style>
        div p{
            background-color: red;
        }
    </style>
</head>
<body>
    <div>
        <p>你好</p>
        <p>你好</p>
    </div>
    <p>你好</p>
</body>
```

> 运行
![结果](css1.png)
只有`<div>`中的`<p>`设置了红色背景

* 父子关系
```html
element1>element2
```
> 选取包含于`element1`中的`element2`元素，只选取子级，孙级及以下不选取


* 相邻关系
```html
element1+element2
```
> 选取`element1`后面紧跟的第一个`element2`元素


* 兄弟关系
```html
element1~element2
```
> 选择`element1`之后出现的所有`element2`,两种元素必须拥有相同的父元素，但是`element2`不必直接紧随`element1`



### 属性选择器
* 只要包含某一个属性
```html
element[attribute]
```

> 用于选取`element`带有`attribute`属性的元素

* 选取定值属性
```html
element[attribute=value]
```

> 用于选取`element`带有`attribute`属性且它的值为`value`的元素

* 选取包含某值的属性
```html
[attribute~=value]
```

> 用于选取`element`带有`attribute`属性且它的值包含`value`元素，如`title~=abc`，则`title`中所有带有`abc`的都会被选中

* 选取以整个单词开头的属性
```html
[attribute|=value]
```

> 用于选取`element`带有`attribute`属性且它的值以`value`开头的元素，如`title|=abc`，则`title`中所有以`abc`开头的都会被选中,该值必须是整个单词


* 选取以某个团素开头的属性
```html
[attribute^=value]
```

> 用于选取`element`带有`attribute`属性且它的值以`value`开头的元素，如`title^=abc`，则`title`中所有以`abc`开头的都会被选中


* 选取以某个元素结尾的属性
```html
[attribute$=value]
```

> 用于选取`element`带有`attribute`属性且它的值以`value`结尾的元素，如`title$=abc`，则`title`中所有以`abc`结尾的都会被选中


* 选择包含某个元素的属性
```html
[attribute*=value]
```

> 用于选取`element`带有`attribute`属性且它的值包含`value`，如`title*=abc`，则`title`中所有包含`abc`的都会被选中

