---
title: C++STL
categories: C++学习笔记
---

标准模板库（英语：Standard Template Library，缩写：STL），是一个C++软件库，大量影响了C++标准程序库但并非是其的一部分。其中包含4个组件，分别为算法、容器、函数、迭代器

- 参考C++STL源码剖析
<!--more-->

# 分配器
## 简介

由于`C++`中存在有`list`，`vector`，`string`等大小会改变的容易因此需要使用分配器对内存空间进行分配和管理。

满足以下需求的类都可以成为分配器
```cpp
A::pointer //指针
A::const_pointer// 常量指针
A::reference //引用
A::const_reference //常量引用
A::value_type //值类型
A::size_type //所用内存大小的类型，表示类A所定义的分配模型中的单个对象最大尺寸的无符号整型
A::difference_type //指针差值的类型，为带符号整型，用于表示分配模型内的两个指针的差异值
```

## 设计一个分配器
```hpp

```
