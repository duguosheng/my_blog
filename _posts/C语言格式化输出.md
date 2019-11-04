---
title: C语言格式化输出
date: 2019-10-23 23:06:18
tags:
- 格式化输出
categories:
- C语言
description:
- 格式化输出的函数有printf、sprintf和snprintf等
---

<!--more-->


## 格式说明符
```C
%[flags][width][.prec]type
```

### 类型符type
| 说明         | 标识符 | 含义       | 标识符 | 含义                 | 标识符 | 含义   |
|--------------|--------|------------|--------|----------------------|--------|--------|
| 十进制有符号 | %hd    | short      | %d     | int                  | %ld    | long   |
| 十进制无符号 | %hu    | short      | %u     | int                  | %lu    | long   |
|              | %c     | 输出字符   | %f     | float                | %lf    | double |
|              | %s     | 输出字符串 | %e     | 科学计数法输出double |        |        |

### 宽度width
```C
print("=%5s=\n", "abc");  //输出=  abc=
```

### 对齐标志flags
* `+` 左对齐
* `-` 右对齐
* 缺省为`+`

```C
print("=%-5s=", "abc");  //输出=abc  =
```

* 如果输出整数或浮点数可在左补0
```C
printf("=%05d=", 123);  //输出=00123=
```

### 精度prec
* 如果输出是浮点数，它用于控制输出内容的精度，即保留位数，后面的四舍五入
```C
printf("=%010.2lf=", 123.456);  //输出=0000123.46=
```
## 格式化输出到字符串
```C
int printf(const char *format, ...);
int sprintf(char *str, const char *format, ...);
int snprintf(char *str, size_t size, const char *format, ...);
```

是参数个数可变的函数

功能：printf是把结果输出到屏幕，sprintf把格式化输出的内容保存到字符串str中，snprintf的n类似于strncpy中的n，意思是只获取输出结果的前n-1个字符，不是n个字符。

在之前的章节中，介绍过把字符串转换为整数和浮点数据的库函数，C语言没有提供把整数和浮点数据转换为字符串的库函数，而是采用sprintf和snprintf函数格式化输出到字符串。

```C
char str[300];
sprintf(str, "%s今年%d岁了\n", "小明", 18);
printf("%s", str);
snprintf(str, 11, "%s今年%d岁了", "小明", 18);
printf("%s", str);
```

> 输出结果
```
小明今年18岁了
小明今�
```

> snprintf截取中文不当会乱码

## 一个例子
```C
#include <stdio.h>
#include <string.h>

/**
 * @func: 解析xml文件
 * @param: in_XMLBuffer所要查询的段落，in_FieldName所要查询的信息，out_Value获取内容存放的变量的指针
 * @return: 0-成功，-1-失败
 */
int GetXMLBuffer(const char *in_XMLBuffer,const char *in_FieldName,char *out_Value)
{
    char begin_filed_name[strlen(in_FieldName)+2];
    char end_filed_name[strlen(in_FieldName)+3];
    memset(begin_filed_name, 0, sizeof(begin_filed_name));
    memset(end_filed_name, 0, sizeof(end_filed_name));
    sprintf(begin_filed_name, "<%s>", in_FieldName);
    sprintf(end_filed_name, "</%s>", in_FieldName);

    char *start, *end;
    start = end = 0;
    //寻找开始和结束地址
    start = strstr(in_XMLBuffer, begin_filed_name);
    end = strstr(in_XMLBuffer, end_filed_name);

    if(start==0 || end==0)
    {
        printf("%s:not found\n", in_FieldName);
        return -1;
    }
    
    int len = end-start;
    strncpy(out_Value, start+strlen(begin_filed_name), end-start-strlen(begin_filed_name));
    return 0;
}
/*
int main()
{
    char str_XML_buffer[301], str_value[51], str_age[51];
    memset(str_XML_buffer, 0, sizeof(str_XML_buffer));
    memset(str_value, 0, sizeof(str_value));
    strcpy(str_XML_buffer, "<name>西施</name><age>18</age><sc>火辣</sc><yz>漂亮</yz>");
    GetXMLBuffer(str_XML_buffer, "name", str_value);
    GetXMLBuffer(str_XML_buffer, "nme", str_value);
    GetXMLBuffer(str_XML_buffer, "age", str_age);
    printf("%s", str_value);
    printf("%s", str_age);
}
*/
```

