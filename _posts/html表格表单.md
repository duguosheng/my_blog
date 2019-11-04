---
title: html表格表单
date: 2019-10-03 10:33:10
tags:
- html基础
categories:
- html
description: 表单表格
---

<!--more-->

## 表格
* `<caption>`标签：表格标题
* `<table>`标签：声明一个表格，它的常用属性如下
  * `bgcolor`：定义背景色
  * `background`：背景图片
  * `border`：定义表格的边框，设置值是数值
  * `bordercolor`：定义表格的边框的颜色
  * `width`：表格宽度
  * `height`：表格高度
  * `cellpadding`：定义单元格内容与边框之间的间距，设置值是数值
  * `cellspacing`：定义单元格与单元格之间的间距，设置值是数值
  * `align`：设置整体表格相对于浏览器窗口的水平对齐方式，设置值有`left`，`center`，`right`
* `<tr>`标签：定义表格中的一行
* `<td>`和`<th>`标签：定义一行中的一个单元格，`<td>`表示普通单元格，`<th>`表示表头单元格，常用属性如下
  * `align`：设置单元格中的内容水平对齐方式，设置值有`left`，`center`，`right`
  * `valign`：设置单元格中内容垂直对齐方式：设置值有`top`，`middle`，`bottom`
  * `colspan`：设置单元格水平合并，设置值是数值
  * `rowspan`：设置单元格垂直合并，设置值是数值
  * `bgcolor`：设置背景色


## 表单
> 用于收集不同类型的用户输入
#### form标签
* 定义整体的表单区域
>  `action`属性定义表单数据提交地址
```html
<body>
    <h2>登陆界面</h2>
    <form action="success.html">    <p>请输入帐号：<input type="text" placeholder="请输入手机号或者邮箱"></p>
    <p>请输入密码：<input type="password"></p>
    <input type="submit" value="登录"></p>
    <input type="reset"></p>
    </form>
</body>
```
另外在当前目录下新建一个`success.html`显示内容为**欢迎你**
> 运行

![action](ht_3.png)
点击登录
![登录](ht_4.png)

成功跳转

> `method`属性定义表单提交的方式，一般有`get`和`post`方式
`post`方法通常用于提交数据，数据的内容不会显示在浏览器的地址栏中，且对数据的长度没有限制。
`get`方法会将你的数据在浏览器地址栏中显示出来，而且由于URL长度有限，所以传递的数据长度也受限制。

#### `<input>`标签：
> `type`属性：
  type="text"：输入文本
  type="password"：输入密码
  type="submit"：提交表单，显示文字默认是**提交**，可以使用`value=""`更改
  type="reset"：清空数据，显示文字默认是**重置**，可以使用`value=""`更改
  type="botton"：自定义按键
  type="checkbox"：多选框
  type="radio"：单选框，必须有`name`属性，且`name`值必须相同
  type="hidden"：隐藏
  type="file"：上传文件
> h5新增：
  type="date"：输入日期
  type="week"：输入第几周
  type="url"：输入网址
  type="email"：输入邮箱
  type="cloor"：输入颜色
  type="range"：还需要设定最小值，最大值，是一个拖动条
  type="search"：搜索内容

> `name`属性：为输入内容定义名字

> `checked`属性：布尔类型，在某个单选或复选框内加入表示默认选择

> `required`属性：布尔类型，加上它表示此项必须要填

> `readonly`属性：布尔类型，只读，表示只能查看，不能修改

> `value`属性：
存储填入的值
* `placeholder`：提示信息
```html
<p>请输入帐号：<input type="text" placeholder="请输入手机号或者邮箱"></p>
```

> 运行结果
![提示信息](ht_1.png)

#### 下拉列表
* `<select>`标签：定义下拉框区域
> `size`属性：定义显示的下拉菜单中的数量

> `name`属性：定义名称
* `<option>`标签：下拉框中的选项
> `selected`属性：布尔类型，表示缺省选择这个选项

* `<optgroup>`标签：定义下拉框组，里面继续嵌套`<option>`

#### 文本框
* `<textarea>`标签:
> `rows`：定义行数
> `cols`：定义列数
* `placeholder`：提示信息

#### label标签
> 一般可以给如上面的*请输入帐号*加一个`<label>`标签，无实际效果，只是为了后面加样式
```html
<label>请输入帐号</label><input name="id" type="text">
```

```html
<body>
    <h1>注册表单</h1>
    <form action="">
        <p>
        <label for="">用户名：</label>
        <input type="text" name="" />
        </p>

        <p>
        <label for="">密&nbsp;&nbsp;&nbsp;&nbsp;码：</label>
        <input type="password" name="" />
        </p>

        <p>
        <label for="">性&nbsp;&nbsp;&nbsp;&nbsp;别：</label>
        <input type="radio" name="gender" /> 男
        <input type="radio" name="gender" /> 女
        </p>

        <p>
        <label for="">爱&nbsp;&nbsp;&nbsp;&nbsp;好：</label>
        <input type="checkbox" name="hobby" /> 看电影
        <input type="checkbox" name="hobby" /> 学习
        <input type="checkbox" name="hobby" /> 读书
        </p>

        <p>
        <label for="">照&nbsp;&nbsp;&nbsp;&nbsp;片：</label>
        <input type="file" name="photo" />
        </p>

        <p>
        <label for="">描&nbsp;&nbsp;&nbsp;&nbsp;述：</label>
        <textarea name="" id="" cols="30" rows="5"></textarea>
        </p>

        <p>
        <label for="">籍&nbsp;&nbsp;&nbsp;&nbsp;贯：</label>
        <select>
            <option>北京</option>
            <option>上海</option>
            <option>天津</option>
            <option>重庆</option>
        </select>
        </p>

        <p>
            <input type="submit" value="提交">
            <input type="reset" value="重置">
        </p>
    </form>
</body>
```

> 运行结果
![表单](ht_2.png)




