---
title: C语言目录操作
date: 2019-10-31 23:11:31
tags:
- 目录操作
categories:
- C语言
description:
- 主要使用的是创建目录和列出目录树
---

<!--more-->

* 修改自[码农有道的博客](https://blog.csdn.net/wucz122140729/article/details/98436646)

### 获取当前目录
在系统命令行下我们可以直接输入命令`pwd`来获取当前的工作目录，在C语言中可以使用`getcwd`函数来获取当前工作目录
* 函数声明
```C
//包含于<unistd.h>中
char * getcwd(char * buf,size_t size)
```

* 示例程序
```C
#include <stdio.h>
#include <string.h>
#include <unistd.h>

int main()
{
    char strpwd[301];
    memset(strpwd, 0, sizeof(strpwd));
    getcwd(strpwd, 300);
    printf("当前目录：%s\n", strpwd);
}
```

### 切换目录
函数声明
```C
int chdir(const char *path);
```

> 就像我们在shell里使用cd命令来切换目录一样，在程序里则可以使用chdir系统调用来实现目录的变更。返回值：0-切换成功；非0-失败。
> 切换目录只是切换程序的运行环境目录，而不会改变用户的目录

### 目录的创建和删除
 在系统命令行下我们可以通过mkdir和rmdir命令通过shell来创建一个目录和删除一个目录,在C语言中
* 函数声明
```C
//创建目录,返回0为成功，-1为失败
int mkdir(const char *pathname, mode_t mode);//mode是权限设置，如00755
//删除目录
int rmdir(const char *pathname);
```


### 获取目录中的文件列表
获取目录中的文件列表，类似于ls命令
在实际开发中，我们经常要处理文件，文件是存放在目录中的，在处理文件之前，必须先知道目录中有哪些文件，所以要获取目录中的文件列表。涉及到的库函数如下：

#### 函数声明
```C
// 打开目录的函数opendir的声明。
DIR *opendir(const char *pathname);
//读取目录的函数readdir的声明。
struct dirent *readdir(DIR *dirp);
//关闭目录的函数closedir的声明。
int closedir(DIR *dirp);
```

#### 数据结构
DIR是目录指针，就像文件操作时的文件指针。
调用一次readdir，返回结构体struct dirent（在dirent.h中声明，程序员只管用就行了）的指针，存放本次读取到的文件的信息，就像文件操作时调用一次fgets一样，但是fgets调用获取的内容是一个字符串，readdir返回的是结构体。
```C
struct dirent
{
    long d_ino;                    // inode number 索引节点号
    off_t d_off;                    // offset to this dirent 在目录文件中的偏移
    unsigned short d_reclen;       // length of this d_name 文件名长
    unsigned char d_type;          // the type of d_name 文件类型
    char d_name [NAME_MAX+1];  // file name文件名，最长255字符
};
```

我们只需要关注结构体的d_type和d_name成员，其它的不必关心。
d_name文件名或目录名。
d_type描述了文件的类型，有多种取值，最重要的是8和4，8-常规文件（A regular file）；4-目录（A directory），其它的暂时不关心.

* 示例程序：获取某个目录下的文件和目录
```C
#include <stdio.h>
#include <dirent.h>

//需要输入一个目录参数
int main(int argc, char *argv[])
{
    DIR *dir;
    //打开目录
    if((dir=opendir(argv[1]))==0)
        return -1;
    //用于存放读取到的文件和目录信息
    struct dirent *stdinfo;

    while(1)
    {
        if((stdinfo=readdir(dir))==0)
            break;
        printf("name=%s;type=%d\n", stdinfo->d_name, stdinfo->d_type);
    }
    closedir(dir);
}
```

* 示例程序：获取目录下的文件及子目录下的文件
```C
#include <stdio.h>
#include <string.h>
#include <dirent.h>

int ReadDir(const char *path);

int main(int argc, char *argv[])
{
    //列出目录及子目录下的文件
    ReadDir(argv[1]);
}

int ReadDir(const char *path)
{
    DIR *dir;  //定义目录指针
    char child_path[256];  //子目录的全路径

    //打开目录
    if((dir=opendir(path))==0)
        return -1;

    //存放从目录读到的文件和目录信息
    struct dirent *stdinfo;

    while(1)
    {
        if((stdinfo=readdir(dir))==0)
            break;

        //忽略隐藏文件
        if(strncmp(stdinfo->d_name, ".", 1)==0)
            continue;

        if(stdinfo->d_type==4)
        {
            //child_path = stdinfo->d_name;
            sprintf(child_path, "%s/%s", path, stdinfo->d_name);
            ReadDir(child_path);
        }
        else if (stdinfo->d_type==8)
        {
            printf("%s/%s\n", \
                    path, stdinfo->d_name);
        }
    }
    closedir(dir);
    return 0;
}
```

### 创建目录
#### mkdir函数

#### access库函数
* access用于判断当前操作系统下用户对文件或目录的存取权限
* 包含头文件`<unistd.h>`
* 函数声明
```C
int access(const char *pathname, int mode);
```

pathname文件名或目录名，可以是当前目录的文件或目录，也可以列出全路径。
mode 需要判断的存取权限。在头文件unistd.h中的预定义如下：
```C
#define R_OK 4     // R_OK 只判断是否有读权限
#define W_OK 2     // W_OK 只判断是否有写权限
#define X_OK 1     // X_OK 判断是否有执行权限
#define F_OK 0     // F_OK 只判断是否存在
```

返回值
当pathname满足mode的条件时候返回0，不满足返回-1
在实际开发中，access函数主要用于判断文件或目录是否是存在。

* 示例程序：创建目录(可多级创建)
```C
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/stat.h>

int mkdir_r(const char *path);

int main(int argc, char *argv[])
{
    if(argc!=2)
    {
        printf("参数个数错误\n");
        printf("这是一个用于创建目录的函数，可多级创建\n");
        printf("示例： ./mkdir_r /a/b/c\n");
        return -1;
    }
    mkdir_r(argv[1]);
    return 0;
}

int mkdir_r(const char *path)
{
    char path_name[301];
    for(int i=1; i<strlen(path); i++)
    {
        //检测间隔符‘/’或字符串末尾
        if(path[i]!='/' && path[i+1]!='\0')
            continue;
        memset(path_name, 0, sizeof(path_name));

        if(path[i+1]=='\0')
            strncpy(path_name, path, i+1);
        else
            strncpy(path_name, path, i);

        //判断目录是否存在
        if(access(path_name, F_OK)==0)
        {
            printf("文件path_name=%s已存在\n", path_name);
            continue;
        }

        printf("创建文件path_name=%s\n", path_name);

        if(mkdir(path_name, 00755)==-1)
        {
            printf("创建文件path_name=%s失败\n", path_name);
            return -1;
        }
    }
    return 0;
}
```

