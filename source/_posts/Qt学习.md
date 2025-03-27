---
title: Qt学习
categories: C++
---

我真的不想学 Qt

<!-- more -->

# 槽函数的连接

> C++这块不如 Pyqt 一根

```cpp
public slots: // 槽函数这块要加这个
    void playPushButtonClicked();
    void moreInfoPushButtonClicked();
    void closePushButtonClicked();

/*
其他代码
*/

connect(ui->closePushButton, SIGNAL(clicked()), this, SLOT(closePushButtonClicked())); //槽函数的连接,写到构造函数里
```

# Qt lambda 实现槽函数

```C++
[捕获列表](形参列表) mutable 异常列表 -> 返回类型
{
    函数体
}
```

> 返回类型可以省略 编译器会自动推导
> 有三种捕获 值捕获,引用捕获,隐式捕获
>
> 1，值捕获，即使在 lambda 后面改变了该值，在调用 lambda 时，这个值还是捕获时的值。
> 2，引用捕获，在 lambda 后面改变了该值，在调用 lambda 时，这个值不是捕获时的值，而是改变后的值。
> 3，隐式捕获：[=]代表全部采用值捕获,[&]代表全部采用引用捕获,[=, &val]代表 val 为引用捕获，其余为值捕获,[&,val]代表 val 为值捕获，其余为引用捕获

# Qt 基础知识

## 槽函数的三种写法

1. Qt4 写法
   ```C++
   connect(ui.binOpen,SIGNAL(clicked),this,SLOT(open())); // 写错了编译器不报错
   ```
2. Qt5 写法
   ```C++
    connect(ui.binOpen,&QPushButton::clicked,this,&Widget::open); //写错了编译器会报错，推荐用这种
   ```
3. lambda 表达式写法

   ```C++
   connect(ui.binOpen,&QPushButton::clicked,[=]()){

   }
   ```

## 跨控件实现信号传递

> 以前 pyqt 搞这玩意给我搞累死了
> 一些教程也不好好教人,都发视频了还藏一手故意埋坑故记录一下
> 这个例子我没有用 Clion 去做,而是使用 vscode 手动撸.CMakeLists.txt 文件内容如下

```txt
cmake_minimum_required(VERSION 3.28) # 设置 CMake 的最低版本要求为 3.28
project(EvilMagic)
set(CMAKE_CXX_STANDARD 17) # 设置 C++ 标准为 C++17
set(CMAKE_AUTOMOC ON) # 启用自动处理 Qt 的元对象编译（MOC）
set(CMAKE_AUTORCC ON) # 启用自动处理 Qt 的资源文件编译（RCC）
set(CMAKE_AUTOUIC ON) # 启用自动处理 Qt 的用户界面文件编译（UIC）

find_package(Qt6 COMPONENTS Core Gui Widgets REQUIRED) # 查找并链接 Qt6 的 Core、Gui 和 Widgets 模块

add_executable(
    EvilMagic
    main.cpp
    form.cpp
    child.cpp
    form.h
    child.h
    form.ui
    child.ui
) # 定义可执行文件 QtTest，并列出源文件和用户界面文件

target_link_libraries(EvilMagic Qt::Core Qt::Gui Qt::Widgets) # 链接 Qt6 的 Core、Gui 和 Widgets 模块到可执行文件 QtTest

```

项目路径如下
.
├── CMakeLists.txt
├── build
├── child.cpp
├── child.h
├── child.ui
├── form.cpp
├── form.h
├── form.ui
└── main.cpp
核心点有两个第一个是使用 signal 和 emit 关键字,第二个则是在哪里连接.child.h 的代码如下

```cpp
#ifndef CHILD_H
#define CHILD_H

#include <QWidget>
#include "ui_child.h"
QT_BEGIN_NAMESPACE
namespace Ui
{
    class Child;
}
QT_END_NAMESPACE

class child : public QWidget
{
    Q_OBJECT

public:
    explicit child(QWidget *parent = nullptr);

    ~child() override;

public slots:
    void someEvent();
signals:
    void sendMessageToForm(const QString &message); // 添加信号

private:
    Ui::Child *ui;
};

#endif // CHILD_H
```

child.cpp 的代码如下

```cpp
#include "child.h"

child::child(QWidget *parent) : QWidget(parent),
                                ui(new Ui::Child)
{
    ui->setupUi(this);
    connect(ui->pushButton, &QPushButton::clicked, this, &child::someEvent);
}

child::~child()
{
    delete ui;
}

// 示例：在某个事件中发射信号
void child::someEvent()
{
    emit sendMessageToForm("Hello from child!");
}
```

form.h 以及 form.cpp 的代码如下

```cpp
#ifndef FORM_H
#define FORM_H

#include <QWidget>
#include "ui_form.h"
QT_BEGIN_NAMESPACE
namespace Ui
{
    class Form;
}
QT_END_NAMESPACE

class form : public QWidget
{
    Q_OBJECT

public:
    explicit form(QWidget *parent = nullptr);

    ~form() override;

public slots:
    void receiveMessageFromChild(const QString &message);  // 添加槽函数

private:
    Ui::Form *ui;
};

#endif // FORM_H

```

```cpp
#include "form.h"
#include <QDebug>
form::form(QWidget *parent)
    : QWidget(parent), ui(new Ui::Form)
{
    ui->setupUi(this);
}

form::~form()
{
    delete ui;
}

void form::receiveMessageFromChild(const QString &message)
{
    this->ui->lineEdit->setText(message);
    qDebug() << "Received message from child: " << message;
    // ui->label->setText(message);  // 假设form中有一个label用于显示消息
}

```

## Clion 配置 include 路径

因为 Qt ui 文件对应的 ui_xxx.h 是自动生成的所以一开始没有，尽管你跑代码可以跑出来。但是 clion 不知道，所以你需要告诉他
有这么一个路径。这里我采取的方式是使用 CMakeLists

```txt
include_directories(${CMAKE_BINARY_DIR}/hLayout_autogen/include)
```

其中{CMAKE_BINARY_DIR}是一个内置变量，表示 cmake 在的目录

## 实现窗口重新绘制

```C++
public:
    explicit MP3(QWidget *parent = nullptr);
    void paintEvent(QPaintEvent * event);//就放到public这里就可以了,当你拖动窗口时会自动调用这个函数,让我想到了python的魔法方法
    /*其他代码
    */
```

```C++
    void MP3::paintEvent(QPaintEvent *event) {
    QPainter painter(this);
    painter.drawPixmap(0, 0, width(), height(),
                       QPixmap("/Users/rainbow/workspace/clionProjects/QT6DemoMP3/resource/background.jpg")); //这里我写的是绝对路径
//    qDebug("hello?"); //如果你拖动窗口,你就会发现在不停的输出hello
}
```

> 一些其他的代码,虽然我觉的这样记会把计算机变成一个文科生的工作

```C++
    this->setFixedSize(this->geometry().size()); //设置窗口大小 不允许随便拖动
    this->setWindowFlags(Qt::FramelessWindowHint); //隐藏窗口标题栏
```

关键点有两个 第一个是注意 QtNameSpace 里面的名称,还有就是 QtCreator 创建 ui 文件默认的对象名字是 form 需要修改
