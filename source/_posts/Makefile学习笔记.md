---
title: Makefile学习笔记
categories: 计算机技术
---

# 一个例子

Compile run all cpp files of src

```makefile
CC = clang++
CCFLAGS = -std=c++2b -Wall
TARGET = test
SRCS = $(wildcard ./src/*.cpp)
OBJS = $(patsubst %.cpp,%.o,$(SRCS))
all:$(TARGET)
	mv $(TARGET) ./bin
	mv $(OBJS) ./build
$(TARGET): $(OBJS)
	$(CC) $(CCFLAGS) -o $@ $^

%.o: %.cpp
	$(CC) $(CCFLAGS) -c $< -o $@
.PHONY:clean
clean:
	rm -f bin/* build/*%
```

<!--more-->

# 基础知识

## makefile 的依赖

```makefile
x.txt: m.txt c.txt
	cat m.txt c.txt > x.txt
```

x.txt 依赖 m.txt c.txt 其中 m.txt 是第一个依赖

一条规则的格式为`目标文件: 依赖文件1 依赖文件2 ...`

make 执行时，默认执行第一条规则

## 伪目标

```makefile
.PHONY: clean
clean:
	rm -f m.txt
	rm -f x.txt
```

多数 makefile 中都有.PHONY 这个标识,这意味着 clean 是一个伪目标,假如没有这个标识,大概文件夹中有了 clean 这个文件后,就不会执行`make clean`,make 会认为目标已经完成了.

## 多语句

可以使用`\`把一行语句拆成多行

```Makefile
cd_ok:
	pwd; \
	cd ..; \
	pwd
```

使用`&&`也可以执行多语句,好处是某条语句失败后,就会终止后续的执行

```Makefile
cd_ok:
	cd .. && pwd
```

# 编译 C/C++

## GCC 编译 C/C++的四个过程

- 预处理(pre-processing),E：插入头文件，替换宏，展开宏
  ```shell
  gcc -E main.c -o main.i #.i文件叫做预处理后文件
  ```
- 编译（Compiling）S：编译成汇编
  ```shell
  gcc -S main.i –o main.s #.s汇编文件
  ```
- 汇编（Assembling） c：编译成目标文件
  ```shell
  gcc –c main.s –o main.o #.o就是生成的目标文件了
  ```
- 链接 （Linking）：链接到库中，变成可执行文件
  ```shell
  gcc main.o –o main #可能会有多个目标文件链接成一个可执行文件
  ```
