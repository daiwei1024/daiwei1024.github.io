---
layout:     post
title:      C++编程：细节纠正
subtitle:   
date:       2024-03-19
author:     Daiwei
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - c++
    - cpp
---
# 拷贝

今天遇到了一个 c++ 中常出现的拷贝相关的性能问题:考虑下面代码
```cpp
string str = "aaa";
const string& str2 = str;
auto str3 = str2;
```
在上面的代码中,第三行会触发拷贝构造。那么我们为什么会经常使用auto呢？因为在使用智能指针时,auto并不会引起拷贝构造,因为赋值的右边往往是一个指针,这种情况不需要使用 auto& 来接收。然而我们要时刻谨记,cpp中的对象都是值类型,包括指针,类等。当我们直接进行赋值时都会执行拷贝,使用指针时拷贝的是指针的值。
那么怎么优化上面的代码呢？
```cpp
auto& str3 = str2;
//等效于
string& str3 = str2;
```

## 触发拷贝的情况
1. 赋值运算符
2. 作为参数
3. 作为返回参数的时候

## 解决拷贝的方案
1. 引用
2. 指针
3. 移动语义

移动语义可以通过对象的移动构造函数来实现