---

layout: post
title: c++ 设计条款
category: cPlus
description: c++ 编程设计遵守的条款

---

此文件的框架源于effective c++

# 习惯C++

## 1 看作一个语言联邦

为了理解c++，可以认识主要的次语言：

C

object-oriented C++

template C++

STL

## 2 尽量以const,enum,inline替换 #define

区别：define是预处理器处理的，不被编译器看到。

## 3 尽可能使用const

它告诉编译器某些值不应该改变。

const可以修饰的地方很多，但是并不难：如果const出现在星号左边，表示被指物是常量，如果出现在星号右边，表示指针是常量。

const在面对函数声明时的应用，可以与  函数返回值， 各参数， 函数自身（如果是成员函数）产生关联。

与 函数返回值，和各参数 产生关联时，没有什么特别，就像是局部变量一样。

**const成员函数**

