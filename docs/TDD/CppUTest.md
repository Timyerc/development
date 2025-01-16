# CppUTest

## CppUTest是什么

CppUTest是一个基于C/C++语言的单元（xUnit）测试框架，可以给你的代码做单元测试和测试驱动开发。

### 项目网址

[https://cpputest.github.io/](https://cpputest.github.io/)

### 文档网址

[https://cpputest.github.io/manual.html](https://cpputest.github.io/manual.html)

## 环境安装

目前使用的是[WSL](https://learn.microsoft.com/zh-cn/windows/wsl/)系统环境

### 克隆代码

```
$ git clone https://github.com/cpputest/cpputest.git
```

### 编译并添加到系统环境变量

确保系统已安装更新，并安装以下依赖库：`autoconf, libtool, make, gcc, g++`

```
$ cd cpputest \
$ autoreconf . -i
$ ./configure
$ make tdd
$ export CPPUTEST_HOME=$(pwd)
```

可以把导出环境变量命令`export CPPUTEST_HOME=cpputest目录`添加到`.bashrc`文件里，这样不用每次都设置环境变量。

## 使用方法

进入项目`tests`目录，并运行`make`，将执行所有单元测试。

```
$ bash
$ cd tests
$ make
compiling ioTest.cpp
compiling AllTest.cpp
compiling io.c
Building archive test-lib/liball.a
r - test-obj/../src/driver/io.o
Linking all_tests
Running all_tests
..
OK (2 tests, 2 ran, 2 checks, 0 ignored, 0 filtered out, 0 ms)
```