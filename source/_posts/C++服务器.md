---
title: C++服务器
categories: C++学习笔记
---
Keep it simple stupid
<!--more-->
# Hello Socket
## 套接字起源
>由于每个主机系统都有各自命名进程的方法，而且常常是不兼容的，因此，要在全网范围内硬把进程名字统一起来是不现实的。所以，每个计算机网络中都要引入一种起介质作用的、全网一致的标准命名空间。
>这种标准名字，在ARPA网中称作套接字，而在很多其他计算机网中称作信口。更确切地说，进程之间的连接是通过套接字或信口构成的
>
```cpp
#include <iostream>
#include <sys/socket.h>
int main(int argc, const char * argv[]) {
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    int sockfd2 = socket(AF_INET6, SOCK_STREAM, 0);
    std::cout << sockfd <<" " << sockfd2<<std::endl; // 输出是3 4
    return 0;
}
```

- 第一个参数：IP地址类型，AF_INET表示使用IPv4，如果使用IPv6请使用AF_INET6。
- 第二个参数：数据传输方式，SOCK_STREAM表示流格式、面向连接，多用于TCP。SOCK_DGRAM表示数据报格式、无连接，多用于UDP。
- 第三个参数：协议，0表示根据前面的两个参数自动推导协议类型。设置为IPPROTO_TCP和IPPTOTO_UDP，分别表示TCP和UDP。