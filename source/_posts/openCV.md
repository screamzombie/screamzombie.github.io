---
title: openCV学习
categories: C++学习笔记
---
OpenCV4 is more than OpenCV
<!--more-->
# 配置
```shell
brew install opencv
```
cmake文件
```txt
cmake_minimum_required(VERSION 3.28)
project(learnOpenCV) # 工程名
set(CMAKE_CXX_STANDARD 20)
find_package(OpenCV)
message(STATUS "OpenCV include directories: ${OpenCV_INCLUDE_DIRS}")
include_directories(${OpenCV_INCLUDE_DIRS})
add_executable(learnOpenCV main.cpp) # main是工程的主文件
target_link_libraries(learnOpenCV ${OpenCV_LIBS})
```

测试代码
```cpp
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

int main() {
    Mat srcImage = imread("images.jpg");
    if (!srcImage.data) {
        std::cout << "Image not loaded";
        return -1;
    }
    imshow("image", srcImage);
    waitKey(0);
    return 0;
}
```