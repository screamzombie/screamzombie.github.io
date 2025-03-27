---
title: Python3
categories: Python3
---

人生苦短,我用 Python

<!-- more -->

# Python 进阶

## 多线程技术

### Python 对于并发编程的支持

1. 多线程:`threading` 利用 CPU 和 IO 可以同时执行的原理,让 CPU 不会干等待 IO 完成
2. 多进程:`multiprocessing`,利用多核 CPU 的能力,真正的并行执行任务
3. 异步 ID:`asyncio`,在**单线程**利用 CPU 和 IO 同时执行的原理,实现函数异步执行

4. 使用`Lock` 对资源加锁,防止冲突访问
5. 使用`Queue`实现不同线程/进程之间的数据通信,实现生产者消费者模式
6. 使用线程池`Pool`/进程池`Pool`,简化线程/进程的任务提交,等待结束,获取结果

### Python 并发编程的三种方式

1. 多线程 `Thread`
2. 多进程 `Process`
3. 多协程 `Coroutine`

> 一个`进程`中可以启动多个`线程`,一个`线程`中又可以启动多个`协程`

#### 对比

**多进程 Process(multiprocessing)**
优点:可以利用多核 CPU 并行计算
缺点:占用资源最多,可启动数目比线程少
适用于 CPU 密集型计算

**多线程 Thread(threading)**
优点:更加轻量
缺点:相比进程,多线程只能并发执行,不能利用多 CPU(GIL),相比协程启动数目有限制,占用内存资源,有切换开销
适用于 IO 密集型计算

> 即在 Python 中 多线程只能使用一个 CPU

**多协程 Coroutine (asyncio)**
优点:内存开销最少,启动协程数量最多
缺点:目前库有限制,代码实现复杂
适用于 IO 密集型计算,需要超多任务运行,但有线程库支持的场景

### Python 全局解释器锁 GIL (Global Interpreter Lock)

是 Python 用于同步线程的一种机制,使得任何时刻仅有一个线程在执行

**如何应对 GIL**

1. 多线程 `threading`请多用于`IO密集型计算`,因为在`I/O`期间,线程会释放 GIL.但是如果多线程用于`CPU密集型计算`只会更加拖累速度
2. 使用`multiprocessing`的多进程机制实现并行计算,利用多核 CPU 优势
