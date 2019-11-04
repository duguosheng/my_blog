---
title: css伪类标签
date: 2019-10-12 21:35:00
tags:
- css基础
categories:
- css
description:
- css的伪类标签
---

<!--more-->

| 伪类标签                             | 含义                                                                           |
|--------------------------------------|--------------------------------------------------------------------------------|
| `a:link`                             | 普通的、未被访问的链接                                                         |
| `a:visited`                          | 用户已访问的链接                                                               |
| `a:hover`, `p:hover`, `#id:hover`    | 鼠标指针位于链接的上方                                                         |
| `a:active`, `p:active`, `#id:active` | 链接被点击的时刻                                                               |
| `E:not(#p1)`                         | 所有的`<E>`标签，除了`id`为`p1`的                                              |
| `E:first-child`                      | 父类的第一个子级，如`li:first-child`表示选择所有作为第一个子级的`li`           |
| `E:last-child`                       | 父类的最后一个子级，如`li:last-child`表示选择所有作为最后一个子级的`li`        |
| `E:only-child`                       | 只有一个子级，如`li:only-child`表示选择`li`，且它的同级必须只有一个`li`        |
| `E:empty`                            | 匹配为空的`<E>`标签                                                            |
| `E:checked`                          | 匹配用户界面上处于选中状态的元素`<E>`，用于`input type`为`radio`与`checkbox`时，或`<option>` |


| 伪对象标签  | 作用                |
|-------------|---------------------|
| `E::before` | 在`<E>`前面添加内容 |
| `E::after`  | 在`<E>`后面添加内容 |

* 当为链接的不同状态设置样式时，请按照以下次序规则：
> a:hover 必须位于 a:link 和 a:visited 之后
> a:active 必须位于 a:hover 之后

* first-child实例
```html
<head>
    <style>
        /*注意不是ul:first-child*/
        li:first-child{
            background-color: gold;
        }
    </style>
</head>
<body>
    <ul>
        <li>abc1</li>
        <li>abc2</li>
        <li>abc3</li>
        <li>abc4</li>
    </ul>
</body>
```

> 效果
![效果](css1.png)

* checked实例
```html
<head>
    <style>
        option:checked{
            background-color: red;
        }
        /* +表示同级 */
        input:checked+span{
            color: red;
        }
    </style>
</head>
</body />
```

> 效果
![效果](css2.png)

* 伪对象标签
```html
<head>
    <style>
        p::before{
            content: "你好";
            color: red;
        }
    </style>
</head>
<body>
    <p>世界</p>
</body>
</html>
```

> 效果
![效果](css3.png)

