---
title: wordPress文章版本
date: 2019-09-22 19:06:00
tags:
- 文章版本
- wordPress配置
categories:
- wordPress
description: wordPress默认对文章的每次修改都会存为一个版本，可以在配置文件中更改
---

<!--more-->

修改站点的`wp-config.php`文件，在其中添加如下代码
```php
define('WP_POST_REVISIONS', 0);     //不启用修订版本
define('WP_POST_REVISIONS', 3);     //共保存3个版本
```

