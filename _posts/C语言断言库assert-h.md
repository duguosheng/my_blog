---
title: C语言断言库assert.h
date: 2020-04-02 10:42:57
tags:
- 断言库
categories:
- C语言
description:
- assert.h是一个用于辅助调试程序的小型库
---

<!--more-->
### assert宏
`assert()`宏接收一个整型表达式作为参数，如果表达式为假，就在标准错误`stderr`中写入一条错误信息，如测试名，文件名，行号等，并调用`abort()`终止程序

* 示例

```C
#include <stdio.h>
#include <assert.h>
int Div(int a, int b)
{
    assert(b != 0);
    return a / b;
}
int main()
{
    printf("%d\n", Div(5, 0));
}
```

假设程序名为`test.c`，编译出的可执行文件为`test`,运行程序
```
test: test.c:5: Div: Assertion `b != 0' failed.
```

执行test文件，其源文件是test.c，第5行，b!=0的测试未通过

* 禁用assert
如果认为已经排除了bug，可以关闭`assert()`，断言库提供了无须更改代码就可开闭`assert`的机制
通过在**导入`<assert.h>`之前加上如下的宏定义，可以关闭`assert`断言**
```C
//此宏定义必须在导入assert.h之前
#define NDEBUG
#include <assert.h>
```


### _Static_assert(C11)
`assert`是在运行时检查，C11新增`_Static_assert`声明，可以在编译时检查`assert()`表达式，`assert`会导致程序的终止，`_Static_assert`会导致编译的失败
`_Static_assert`接收两个参数，第一个参数是整型常量表达式，第二个参数是一个字符串，如果第一个表达式求值为假(0或`_False`)，编译器会显示字符串，且不通过编译

* 示例
```C
#include <assert.h>
#include <limits.h>
#include <stdio.h>
_Static_assert(CHAR_BIT == 16, "char不是16位");
int main()
{
    printf("OK\n");
}
```

假设该文件名为`test.c`，使用gcc编译它
```bash
$ gcc -o test test.c
test.c:4:1: 错误：静态断言错误："char is not 16bits"
    4 | _Static_assert(CHAR_BIT == 16, "char is not 16bits");
      | ^~~~~~~~~~~~~~
```
