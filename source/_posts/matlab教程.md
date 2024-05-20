---
title: MATLAB教程
categories: 计算机技术
---

# MATLAB 的安装

MATLAB 是一个收费软件并且价格对学生党来说过于昂贵。这里使用的是一个破解版的。

安装的方式很简单，基本在教程里都有说了。下面是软件的下载链接

```
链接：https://pan.baidu.com/s/1K0RDDru1wVK92CMbwRL_NQ
提取码：bu3f
复制这段内容后打开百度网盘手机App，操作更方便哦--来自百度网盘超级会员V4的分享
```
<!-- more -->
# 勇敢的第一步

## 使用 MATLAB 进行第一个运算

创建变量

```
a = 1;
b = [1,2,3;4,5,6;7,8,9];
```

当不指定输出变量时，MATLAB 使用变量 ans （answer 的缩写）存储计算结果。

如果以英文分号（;）结束语句，MATLAB 会执行计算，但会在命令窗口中隐藏对应的输出值。

创建矩阵的另一种方法是使用函数，如产生一组 1、0 或随机数。例如，创建一个由 0 组成的 5×1 列向量

```
z = zeros(5,1)

z = 5×1
	0
	0
	0
	0
	0
```

## 矩阵运算

```
a = [1 2 3; 4 5 6; 7 8 10]
a + 10
sin(a)
```

上面这段代码运行的结果是

```
ans =

   0.841470984807897   0.909297426825682   0.141120008059867
  -0.756802495307928  -0.958924274663138  -0.279415498198926
   0.656986598718789   0.989358246623382  -0.544021110889370
```

注意在 MATLAB 中如果式子的值没有返回给一个变量，那么他会保存在一个叫 ans 的自带的变量中

### 转置矩阵

**如果要使用转置矩阵请在变量的右上角加一个'**
例如`a'`

**转置矩阵的定义:**
_将矩阵的行列互换得到的新矩阵称为转置矩阵，转置矩阵的行列式不变_

### 矩阵乘法

使用\*号来完成两个矩阵之间的乘法运算。没错就是我们在《线性代数》中学到的那样

**inv(a)会返回一个矩阵的逆矩阵**

**逆矩阵：**
_设 A 是一个 n 阶矩阵，若存在另一个 n 阶矩阵 B，使得： AB=BA=E ，则称方阵 A 可逆，并称方阵 B 是 A 的逆矩阵。_

**矩阵乘法:**
_矩阵相乘最重要的方法是一般矩阵乘积。它只有在第一个矩阵的列数（column）和第二个矩阵的行数（row）相同时才有意义 。一般单指矩阵乘积时，指的便是一般矩阵乘积。一个 m×n 的矩阵就是 m×n 个数排成 m 行 n 列的一个数阵_

我感觉我已经不认识“矩”这个字了= =

```
a = [1 2 3; 4 5 6; 7 8 10]
p = a*inv(a)
```

### 设置数值格式

在上面这个代码中，你的答案可能是下面两种之一

长这样

```
p =

    1.0000   -0.0000   -0.0000
    0.0000    1.0000   -0.0000
    0.0000   -0.0000    1.0000
```

或者是长这样

```
p =

   1.000000000000000  -0.000000000000000  -0.000000000000000
   0.000000000000001   0.999999999999999  -0.000000000000000
   0.000000000000002  -0.000000000000003   1.000000000000000
```

其中出现第二种情况的原因是，之前有设置过数值格式
使用
在代码的第一行加上

```
format short
```

答案就会变成第一种的样子了。
我们并不需要每次都进行设置，一旦设置过后。它将会被保存下来，直到我们再次更改设置。

**数据格式只影响数字的显示，而不影响 MATLAB 对结果的运算或保存。**

### 矩阵乘法

```
a = [1 2 3; 4 5 6; 7 8 10];
p = a*a
```

结果

```
p =

    30    36    45
    66    81   102
   109   134   169
```

矩阵数乘

```
a = [1 2 3; 4 5 6; 7 8 10];
p = a.*a
```

结果

```
p =

     1     4     9
    16    25    36
    49    64   100
```

## 数组/矩阵的拼合

### 两种不同的拼接方式

在线性代数中我们已经知道了**分块矩阵**的概念。类似的在 MATLAB 中我们也可以把数组进行拼接

**分块矩阵**
_分块矩阵是一个矩阵， 它是把矩阵分别按照横竖分割成一些小的子矩阵 。 然后把每个小矩阵看成一个元素。_

MATLAB 中有两种不同的数组拼接方式
第一种水平拼接，就是左右拼在一起啦(｡・`ω´･)

仔细想想这个我们创建矩阵的时候使用`,`是不是一个道理呀
把俩个小矩阵看成两个元素就可以

```
a = [1 2 3; 4 5 6; 7 8 10];
p = [a,a]
```

结果

```

p =

     1     2     3     1     2     3
     4     5     6     4     5     6
     7     8    10     7     8    10

```

第二种则是竖直拼接，也就是上下拼在一起啦

```
a = [1 2 3; 4 5 6; 7 8 10];
p = [a;a]
```

结果是

```
p =

     1     2     3
     4     5     6
     7     8    10
     1     2     3
     4     5     6
     7     8    10
```

## 复数的表示

我相信各位大佬复数的定义都没有忘记

```
sqrt(-1)
```

```
ans =

   0.0000 + 1.0000i
```

如果想要表示数的虚部可以使用 `i` 或`j`。
像这样

```
c = [3+4i, 4+3j; -i, 10j]
```

```
c =

   3.0000 + 4.0000i   4.0000 + 3.0000i
   0.0000 - 1.0000i   0.0000 +10.0000i
```

使用 i 或者使用 j，不会有任何不同。完全看个人喜好

## 从数组中进行搜索

在过去所学的计算机语言中我们已经接触过了二维数组，它们往往有很方便的得到某一行某一列所有元素的方法比如

C++

```c++
#include <iostream>
using namespace std;
int main()
{
    int numbers[3][3] = {0};
    for(auto i=0;i<3;i++)
       cout << numbers[2][i] << endl;
    return 0;
}
```

Python

```python
ls = [[1,2,3],[4,5,6],[7,8,9]]
print(ls[2])
```

Java

```java
package rainbow.demo;

class Main {

  public static void main(String[] args) {
    int[][] array = { { 1, 2, 3 }, { 4, 5, 6 }, { 7, 8, 9 } };
    for (int j : array[2]) {
      System.out.println(j);
    }
  }
}
```

Scala

```scala
def dim2B(rows : Int)={
      val d2  = new Array[ArrayBuffer[Int]](rows)
      for (k <-0 until rows ) {
        d2(k) = new ArrayBuffer[Int]()
      }
      d2
}
val index = dim2B(2)
```

C#

```Csharp
using System;
namespace Work
{

   class MyMain
    {
        public static void Main()
        {
            int[,] arrray1 = new int[3,3] { {1,2,3}, {4,5,6},{ 7,8,9} };
            for(int i = 0; i < 3; i++)
            {
                Console.WriteLine(arrray1[2,i]);
            }
            Console.ReadLine();
        }
    }

}
```

### 查找某一个元素

```MATLAB
A = [1,2,3;4,5,6;7,8,9];
A(2,1)
```

```MATLAB
ans =

     4
```

### 查找列或者行

**遍历一整行**

```MATLAB
A = [1,2,3;4,5,6;7,8,9]
A(1,:)
```

```
ans =

     1     2     3
```

**遍历一整列**

```MATLAB
A = [1,2,3;4,5,6;7,8,9]
A(:,2)
```

```MATLAB
ans =

     2
     5
     8
```

## 工作空间变量

使用`whos`查看工作区的内容，也就是查看变量

# 输出 Hello World

## 字符串数组中的文本

MATLAB 也支持对文本的操作。通过这样的方式创建一个字符串，或者说将文本分配给变量。

```MATLAB
line = "hello world"
```

如果文本包含双引号，请在变量的定义中使用两个双引号。类似于 C 语言中使用%%输出百分号。

```MATLAB
q = "My ablity is ""Dirty Deeds Done Dirt Cheap"""
```

显示效果

```
q =

    "My ablity is "Dirty Deeds Done Dirt Cheap""

```

## 字符串拼接

使用加号进行字符串拼接，就像你在 JAVA 和 Python 中做到的那样

```
str1 = "I "
str1 + "LOVE YOU"
```

## strlength

还记的 C 语言中我们怎样求一个字符串的长度的嘛？！
使用 string.h 头文件中的 strlen 函数

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main()
{
    char str1[100] ="hello world";
    int length = strlen(str1);
    printf("%d\n",length)l;
    return 0;
}
```

不过在 MATLAB 中我们也有类似的函数 叫 strlength
使用方法和 C 语言的 strlen 十分类似
这里直接举 MATLAB 官方手册的例子

```MATLAB
A = ["a","bb","ccc"; "dddd","eeeeee","fffffff"]

A =
	2×3 string array
	"a"     "bb"      "ccc"
	"dddd"  "eeeeee"  "fffffff"
strlength(A)

ans =
	1 2 3
	4 6 7
1
```

# MATLAB 中的函数

函数是一个我们非常熟悉的概念，甚至后面还出现了函数式编程的语言 **scala**
MATLAB 中的函数有两种 第一种是内置的，第二种是用户自定义的。
囿于篇幅，我们不讨论 MATLAB 自带的函数，想要了解的话可以去官网查看原文文档。
MATLAB 有中文文档。
我们需要进行讨论和学习的是第二种情况

## 如何创建和使用自己定义函数

在别的高级编程语言中，我们要定义一个函数只需要使用如下的语句例如这些
(随便写的没有分行)

```
public static void Myfunction(){
    ·······
}

def my_select_paper_window(self, paper):
    pass

void Say()
{
    ······
}
```

但是在 MATLAB 中情况有了些许不同
我们先创建一个文件叫 myfunction.m 名字可以自己取
创建函数类似这样的写法

```MATLAB
function y = j(x)
    y = x*x+5;
end
```

调用函数必须保证两个文件在同一个目录下

```
t = myfunction(3);
```

# 二维图像和三维图像绘制

## 二维图像

使用`plot` 函数来创建一个二维曲线图。

```matlab
x = 0:pi/100:2*pi;
y = sin(x);
xlabel('x')
ylabel('sin(x)')
title('Plot of the Sine Function')
plot(x,y,'g:*')
```

x 的三个元素分别是 开始的坐标，拐点数量，结束的坐标,plot 中第三个元素表示使用绿线进行标识

## 多个函数图像在一个图中

```matlab
x = 0:pi/100:2*pi;
y = sin(x);
plot(x,y)
hold on
y2 = cos(x);
plot(x,y2,':')
legend('sin','cos')
hold off
```

使用 hold on ，握住这个图像，然后往里面添加新的图像。hold off 松手之后图像一齐绘制出来

## 三维图像

早在《高等数学》中我们就已经接触过多元函数的概念了。
比较常见的是由 x,y,z 组成的一个三维空间内的曲面
使用 MATLAB 可以很方便的帮我们绘制图像
接下来是一段代码

```MATLAB
[X,Y] = meshgrid(-2:.2:2);
Z = X .* exp(-X.^2 - Y.^2);
surf(X,Y,Z)
```

### 注解

**meshgrid**
[X,Y] = meshgrid(x,y)
[X,Y] = meshgrid(x)
[X,Y,Z] = meshgrid(x,y,z)
[X,Y,Z] = meshgrid(x)
**Description**
_example_
[X,Y] = meshgrid(x,y) returns 2-D grid coordinates based on the coordinates contained in vectors x and y. X is a matrix where each row is a copy of x, and Y is a matrix where each column is a copy of y. The grid represented by the coordinates X and Y has length(y) rows and length(x) columns.

_example_
[X,Y] = meshgrid(x) is the same as [X,Y] = meshgrid(x,x), returning square grid coordinates with grid size length(x)-by-length(x).

_example_
[X,Y,Z] = meshgrid(x,y,z) returns 3-D grid coordinates defined by the vectors x, y, and z. The grid represented by X, Y, and Z has size length(y)-by-length(x)-by-length(z).

_example_
[X,Y,Z] = meshgrid(x) is the same as [X,Y,Z] = meshgrid(x,x,x), returning 3-D grid coordinates with grid size length(x)-by-length(x)-by-length(x).

**_上面的内容是 MATLAB 官方手册中对这个函数的解释_**
说人话就是**用两个坐标轴上的点在平面上画格。**
不理解不要紧我们来看一下这个例子

```MATLAB
x=[-3:1:3];
y=[-2:1:2];
[X,Y]= meshgrid(x,y)
```

运行结果是

```MATLAB
X =

    -3    -2    -1     0     1     2     3
    -3    -2    -1     0     1     2     3
    -3    -2    -1     0     1     2     3
    -3    -2    -1     0     1     2     3
    -3    -2    -1     0     1     2     3


Y =

    -2    -2    -2    -2    -2    -2    -2
    -1    -1    -1    -1    -1    -1    -1
     0     0     0     0     0     0     0
     1     1     1     1     1     1     1
     2     2     2     2     2     2     2
```

仔细看 X 和 Y 。X 是 -3 到 +3，Y 是 -2 到 +2。
是不是有点明白了
再看一段代码

```MATLAB
x=[-3:2:3];
y=[-2:2:2];
[X,Y]= meshgrid(x,y)
```

```MATLAB

X =

    -3    -1     1     3
    -3    -1     1     3
    -3    -1     1     3
Y =

    -2    -2    -2    -2
     0     0     0     0
     2     2     2     2
```

原来中间的数字是 step 啊
所以我们可以说函数的形参规则是[start:step:end]
第一个参数是开始，第二个参数是每次改变的坐标，第三个参数则是在哪截止

**三维空间的情况**
类似的上面这个例子也可以用在三维空间中

```MATLAB
x = [-3:1:3];
y = [-2:1:2];
z = [-4:1:4]
[X,Y,Z] = meshgrid(x,y,z)
```

效果也是和上面的类似，不过是三维的由于答案比较长，就不放在这里了。

最后最后上面这么多说人话就是用 MATLAB 的方式建立去建立一个坐标。
虽然并不是我们所熟悉的笛卡尔坐标系就是了。
**surf 函数**
surf 函数和与之一起使用的 mesh 函数用于在三维空间显示曲面。
surf 函数用于彩色显示出连接线和表面。

**mesh 函数**
mesh 函数用于产生表面的线框，并且只标记的点之间的连线线框着色。

## 子图

使用 subplot 函数在同一个窗口的不同子区域中显示多个绘图。

**语法**

```MATLAB
subplot(m,n,p)  %先只看第一个就好了
subplot(m,n,p,'replace')
subplot(m,n,p,'align')
subplot(m,n,p,ax)
subplot('Position',pos)
subplot(**_,Name,Value)
ax = subplot(_**)
subplot(ax)
```

_subplot(m,n,p) 将当前图窗划分为 m×n 网格，并在 p 指定的位置创建坐标区。MATLAB® 按行号对子图位置进行编号。第一个子图是第一行的第一列，第二个子图是第一行的第二列，依此类推。如果指定的位置已存在坐标区，则此命令会将该坐标区设为当前坐标区。_

示例

```MATLAB
t = 0:pi/10:2*pi;
[X,Y,Z] = cylinder(4*cos(t));
subplot(2,2,1); mesh(X); title('X');
subplot(2,2,2); mesh(Y); title('Y');
subplot(2,2,3); mesh(Z); title('Z');
subplot(2,2,4); mesh(X,Y,Z); title('X,Y,Z');
```

# 程序脚本

## 怎样创建一个脚本？

在主流 linux 系统中我们往往使用如下命令创建一个文件

```SHELL
touch hello.cpp
```

类似的在 MATLAB 的终端中使用

```
edit your_file_name.m
```

这样就创建了一个名字是 your\*file_name.m 的 m 文件
当然了，你可以起别的名字。
\*\*\*人不能 至少不应该\_\*\* 我还是觉的敲代码的程序员更帅气一点。
如果你非要用鼠标去右键创建文件。那也是可以的

## 开始编写我们的脚本

### 怎样输出变量

相信你肯定学过这样那样的输出手段比如

```
std::cout << "我是C++的输出";
System.out.println("我叫java");
print("俺是python")
printf("我是C语言\n");
```

在 MATLAB 中使用`disp()`来输出一个变量

```matlab
t = "灰烬大人,愿您能找到安歇的港湾";
disp(t)
```

终端输出效果是

```
>> hello
灰烬大人，愿您能找到安歇的港湾
```

至于为什么前面有个 hello 那是因为我这个文件名就叫 hello。你起一个**Dark Souls**也可以

有些同学总想搞个大新闻······（子弹滞销帮帮我们）

说不对啊，我一开始没有使用`disp()`它也有输出啊。
那是因为 MATLAB 有多种输出变量的方式，主要包括以下几类：

**第一种:**
语句后面不加分号“；”，这是直接输出数值的比较简单的方法。

**第二种**
disp(a)直接在命令窗口显示 a 变量，这种方法输出和第一种差不多。

**第三种**
fprintf(‘a=%f',a)格式控制输出：

```
a = [1,2,3];
fprintf("a = %d\n",a);
```

## 分支、跳转与循环

我想没人看的出来这个标题名是在 neta 《病毒、枪炮与细菌》这本书吧。

### 代码块

必须要提的概念是代码块，这是几乎所有的编程语言中都有的概念。
先来看这段代码。

```matlab
A = [4 3 1;6 5 4;9 7 9];
B = max(A,[],2);
B2 = B(:)'
C = min(A,[],1);
for i = B2
    for j = C
        if (i==j)
            i
        end
    end
end
```

注意到，每个 if 或者 for 的后面都有一个 end。
没错 end 正是用来区分代码块的标志。整体的代码风格我感觉和 python 很接近。
让我们直接在实例中学习吧：

_求 1 到 100 内所有的偶数和_

```matlab
s = 0;
for i = 2:2:100
    s = s + i;
end
s
```

_打印 2 到 100 所有的素数_

```matlab
for i = 2 : 100
    for j = 2 : 100
        if (~mod(i,j))
           break;
        end
    end
    if(j > (i/j))
        fprintf('%d is prime \n', i);
    end
end
```

_求水仙花数_

```matlab
m=100:999;
m1=rem(m,10);%求个位数
m2=rem(fix(m/10),10); %求十位数
m3=fix(m/100); %求百位数
k=find(m==m1.^3+m2.^3+m3.^3); %find(一维向量) 得出一维向量的下标序号
s=m(k);
s
```

**说明**
| 语句        | 作用                   |
| ----------- | ---------------------- |
| max(A,[],2) | 求矩阵中每一行的最大值 |
| min(A,[],2) | 求矩阵中每一行的最小值 |
| max(A,[],1) | 求矩阵中每一列的最大值 |
| min(A,[],1) | 求矩阵中每一列的最小值 |

## 输入语句

```matlab
n =10;
while true;
    n=input('请输入n=');
    if n==10;
        break;
    end;
    disp("输入错误");
end;
disp("恭喜输入正确");
```

这，妥妥的 Python 啊。

**前有终结，所以泪**