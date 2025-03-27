---
title: openCV
categories: 计算机技术
date: 2024-12-29 22:42
---

OpenCV 启动!

<!-- more -->

# CMake

```bash
cmake_minimum_required(VERSION 3.28)
project(untitled)

set(CMAKE_CXX_STANDARD 17)

find_package(OpenCV)
include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(untitled main.cpp)

target_link_libraries(untitled ${OpenCV_LIBS})
```

# 基础课程

## 图像读取与显示

```cpp

void ImageThreshold(String str) {
    Mat image = imread(str,IMREAD_GRAYSCALE); // 加载灰度图像的选项
    namedWindow("test",WINDOW_FREERATIO); // 这个可以用来重新定义窗口名，并且可以让窗口可以拉伸
    imshow("test",image);
    waitKey(0);
}

int main() {
    String str = "../../material/YaeMiko.jpg";
    ImageThreshold(str);
    destroyAllWindows();
    return 0;
}
```

## 简单的像素操作

```cpp
void QuickDemo::colorSpace_Demo(cv::Mat &img) {
    Mat gray, hsv;
    cvtColor(img, hsv, COLOR_BGR2HSV);
    cvtColor(img, gray, COLOR_BGR2GRAY);
    imshow("HSV", hsv);
    imshow("GRAY", gray);
    imwrite("../../material/HSV.jpg", hsv);
    imwrite("../../material/GRAY.jpg", gray);
}

void QuickDemo::mat_creation_demo(cv::Mat &img) {
    Mat m1, m2;
    m1 = img.clone();
    img.copyTo(m2);
    Mat m3 = Mat::zeros(Size(4, 4), CV_8UC3);
    m3 = Scalar(214, 214, 147);
//    std::cout << m3.rows << " " << m3.cols << std::endl;
//    std::cout << m3 << std::endl;
    imshow("m3", m3);
}

void QuickDemo::pixel_visit_demo(cv::Mat &img) {
    int w = img.cols;
    int h = img.rows;
    auto dims = img.channels(); //这个就是返回通道数
    std::cout << "dims" << dims << std::endl;
    for (auto row = 0; row < h; row++) {
        for (auto col = 0; col < w; col++) {
            if (dims == 1) { // 如果是单通道
                int pv = img.at<uchar>(row, col);
                img.at<uchar>(row, col)= 255 - pv;
            } else if (dims == 3) { //3个通道 彩色图像
                auto bgr = img.at<Vec3b>(row, col);
                img.at<Vec3b>(row, col)[0] = 255 - bgr[0];
                img.at<Vec3b>(row, col)[1] = 255 - bgr[0];
                img.at<Vec3b>(row, col)[2] = 255 - bgr[0];
            }
        }
    }
    imshow("pixel read and write", img);
}
```
