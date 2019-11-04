---
title: css的布局和背景
date: 2019-10-11 17:40:25
tags:
- css基础
categories:
- css
description:
- css的布局和背景
---

<!--more-->
## 布局
> 常用布局格式
```html
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        header{
            height: 100px;
            background-color: goldenrod;
        }
        aside{
            width:30%;
            height: 300px;
            background-color: skyblue;
            display: inline-block;
        }
        article{
            /*这里不要写70%，因为页面有一些其他元素需要一些空间*/
            width:69%;
            height: 300px;
            background-color: blue;
            display: inline-block;
        }
        footer{
            height: 100px;
            background-color: purple;
        }
    </style>
</head>
<body>
    <header></header>
    <aside></aside>
    <article></article>
    <footer></footer>
</body>
```

> 效果
![布局](css1.png)


## 背景
> [详细资料](https://www.w3school.com.cn/css/css_background.asp)
![背景](css2.png)
