---
title: html5新增标签
date: 2019-10-10 16:12:26
tags:
- html5
categories:
- html
description: html5新增标签
---

详细的html教程 [w3school](https://www.w3school.com.cn/)

<!--more-->
## 常见的新增语义标签
`<header>`: 页面头部
`<footer>`: 页脚
`<article>`: 定义页面独立的内容区域
`<aside>`: 定义页面的侧边栏内容
[`<`details`>`](#details): 文档某个部分的细节
`<summary>`: 是detail中的标题
`<figure>`: 独立的文档流，在`<figure>`中定义的内容会自动缩进一部分
`<figcaption>`: 是`<figure>`的标题
`<mark>`: 重点标记，高亮显示
`<nav>`: 导航连接
[`<`ruby`>`](#ruby): 加注释
[`<`datalist`>`](#datalist): 提示框
[`<`meter`>`](#meter): 用来表示范围已知且可度量的内容
[`<`progress`>`](#progress): 进度条，`max`指定最大值，`value`属性指定当前进度
[`<`audio`>`](#audio): 播放音频
[`<`video`>`](#video): 播放视频
[`<`embed`>`](#embed): 嵌入插件，网页
[画布](#画布)


#### details
`<details>`和`<summary>`联用
> `<summary>`默认为*详细信息*
```html
<details>
    <summary>点击查看</summary>
    <p>
        <h5>这是一张动漫图</h5>
        <img src="images/img_1.jpg" width="80px" height="50px" alt="动漫">
    </p>
</details>
```

> 运行
![效果](h1.png)
点击查看后
![效果](h2.png)

#### ruby
* 加拼音，使用`<rt>`标签
```html
<ruby>
    汪<rt>w<ruby>a<rt>_</rt></ruby>ng</rt>
</ruby>
```

> 运行
![注释](h3.png)

#### datalist
* 和`<input>`联用
```html
<!--使用list指定数据来源-->
<input type="search" list="mydata"/>
<!--用id指定名称，不是name-->
<datalist id="mydata">
    <!--使用value指定提示内容-->
    <!--中间的文字是一些其他附属信息-->
    <option value="abc">热度100</option>
    <option value="bcd">热度70</option>
    <option value="emm">热度50</option>
    <option value="bat">热度30</option>
</datalist>
```

> 运行结果

![datalist](h4.png)
当键入`a`时，自动弹出了和`a`相关的提示

#### meter
> 比较大小
```html
0<meter min="0" max="100" value="40"></meter>100
```
> 运行
![meter](h5.png)


#### progress
> 进度条
```html
0<progress max="100" value="80"></progress>100
```
> 运行
![meter](h6.png)

#### audio
* 属性
`src`: 音频地址
`autoplay`: 自动播放
`controls`: 显示控件，比如播放按钮
`loop`: 循环播放
> 可以在开始标签和结束标签之间放置文本内容，这样老的浏览器就可以显示出不支持该标签的信息。
```html
<audio src="someaudio.mp3">
您的浏览器不支持 audio 标签。
</audio>
```

#### video
* 属性部分同`audio`
`width`: 设置宽度
`height`: 设置高度
`poster`: 设置封面

#### embed
> 定义嵌入的内容，比如插件，网页。
```html
<embed src="/i/helloworld.swf" />
```

> 运行
![embed](h7.png)
成功嵌入了网页，并且可以使用网页的功能

#### 画布
`<canvas>`标签用于绘制标签，<canvas> 元素本身并没有绘制能力（它仅仅是图形的容器，必须使用脚本来完成实际的绘图任务。
