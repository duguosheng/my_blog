---
title: html的段落
date: 2019-10-02 16:52:40
tags:
- html断落
categories:
- html
description: html
---

<!--more-->

## html基本结构
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>网页标题</title>
    </head>
    <body>
        网页显示内容
    </body>
<html>
```

## html注释
```html
<!--注释内容-->
```

## 标题标签
* 通过`<h1>`，`<h2>`...`<h6>`来确定标题等级
```html
<h1>这是一级标题</h1>
<h2>这是二级标题</h2>
<h3>这是三级标题</h3>
...
<h6>这是六级标题</h6>
```

## 段落标签
```html
<p>段落内容</p>
```

* 这种方式的段落中首段开头的空格和换行不会识别，段落中的多个空格只识别一个
```html
<p>    这是  一段
内容</p>
```

> 执行结果

![结果](ht_1.png)

### html字符实体

| 字符   | 代码     |
|--------|----------|
| 空格   | `&nbsp;` |
| 大于号 | `&gt;`   |
| 小于号 | `&lt;`   |
| 换行符 | `<br />` |

## html块标签
### <div>标签
> 块元素，表示一块内容，没有具体的语义
* 与`<p></p>`的区别，在`body`中写如下内容
```html
<p>这是<br />
一段   内容</p>
<p>这是<br />
一段   内容</p>
<div>这是<br />
一段   内容
</div>
<div>这是<br />
一段   内容
</div>
```

> 执行结果

![区别](ht_2.png)

> 可以看到，`p`标签两端内容间有空行，即`p`标签是带有格式的，而`div`没有格式

### <span>标签
> 行内标签，表示一行中的一小段内容，没有具体的语义(在做样式的地方会用到)

## 含样式和语义的标签
* `<em>`标签：行内元素，表示语气中的强调词
* `<i>`标签：行内元素，表示专业词汇
* `<b>`标签：行内元素，表示文档中的关键词或者产品名
* `<strong>`标签：行内元素，表示非常重要的内容

## 图片标签
```html
<img src="图片路径 " alt="图片描述" title="鼠标悬浮在图片上显示的文字"/>
```

## 链接标签
```html
<a href="链接地址" title="鼠标悬停时的描述信息" target="_self">文字或图片</a>
```
> `href="#"`：表示跳转到顶部
> `target="_self"`：替换当前页面，不设定时默认为`_self`
> `target="_blank"`：新开一个页面


* 示例
```html
<a href="http://www.baidu.com">点我跳转百度</a>
```

* 还可以使用图片跳转
```html
<a href="http://www.baidu.com"><img src="images/bd.jpg" alt="百度logo"></a>
```

## 列表标签



