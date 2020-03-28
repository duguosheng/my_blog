---
title: C++抽象类子类实例化的析构
date: 2020-03-06 12:03:16
tags:
- 抽象类
- 虚析构
categories:
- C/C++
---

在C++使用抽象类时会面临一个问题，根据其派生类实例出来的对象，如何释放内存，需要用道虚析构，C++将从最底层依次向上调用析构函数
<!--more-->

### 示例
* 定义类
```C++
class Animal {
public:
    virtual void Run() = 0;
    //定义一个虚析构函数，但不能为纯虚函数
    //如果没有visual将不会调用子类的析构
    virtual ~Animal() { cout << "del animal" << endl; }
};
class Cat : public Animal {
    void Run() { cout << "猫步轻悄" << endl; }
    ~Cat() { cout << "del cat" << endl; }
};
```

> 抽象类的析构前要加'virtual'，否则不会调用子类的析构

* 实例化演示
```C++
int main() {
    Animal *ani = new Cat;
    ani->Run();
    delete ani;
    return 0;
}
```

* 输出
```
猫步轻悄
del cat
del animal
```
