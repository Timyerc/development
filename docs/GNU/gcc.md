# GCC 编译指南

## 概述

GCC (GNU Compiler Collection) 是一个功能强大的编译器套件，支持多种编程语言。本文档介绍 GCC 最常用和重要的编译命令及其功能，以及makefile的优化方式。

## 基本编译流程

GCC 的编译过程分为四个阶段：

1. **预处理** - 展开头文件、宏定义等

2. **编译** - 将 C 代码转换为汇编代码

3. **汇编** - 将汇编代码转换为机器代码

4. **链接** - 将目标文件与库文件链接成可执行文件

## 常用命令选项

### 1. 基础编译命令

**编译单个文件，生成 a.out**

gcc main.c

**编译并指定输出文件名**

gcc main.c -o myApp

### 2. 分步编译命令

1.**预处理**：展开头文件、宏等

gcc -E main.c -o main.i

2.**编译**：生成汇编代码

gcc -S main.i -o main.s

3.**汇编**：生成目标文件

gcc -c main.s -o main.o

4.**链接**：生成可执行文件

gcc main.o -o myApp

### 3. 多文件编译

 方法一：分步编译（推荐用于大型项目）

gcc -c main.c -o main.o
gcc -c helper.c -o helper.o
gcc -c utils.c -o utils.o
gcc main.o helper.o utils.o -o myApp

方法二：一次性编译

gcc main.c helper.c utils.c -o myApp

### 4. 头文件和库文件处理

**指定头文件搜索路径**

gcc -I /path/to/headers main.c -o myApp
gcc -I ../include main.c -o myApp

**链接库文件**

gcc main.c -lm -o myApp# 链接数学库
gcc -L /path/to/libs main.c -lmylib -o myApp# 指定库路径并链接

### 5. 调试和优化

**包含调试信息**

gcc -g main.c -o myApp

**优化级别**

gcc -O0 main.c -o myApp        # 不优化（默认，适合调试）
gcc -O1 main.c -o myApp        # 基本优化
gcc -O2 main.c -o myApp        # 高度优化（推荐）
gcc -O3 main.c -o myApp        # 激进优化
gcc -Os main.c -o myApp        # 优化代码大小
gcc -Ofast main.c -o myApp   # 最高级别的优化（可能不严格遵循标准）

### 6. 警告控制

**显示警告信息**

gcc -Wall main.c -o myApp# 开启常用警告
gcc -Wall -Wextra main.c -o myApp# 开启更多警告
gcc -Wall -Werror main.c -o myApp# 将警告视为错误

### 7. 宏定义

**在命令行中定义宏**

gcc -DDEBUG main.c -o myApp# 相当于在代码中添加 #define DEBUG
gcc -DVERSION=100 main.c -o myApp# 定义带值的宏

# 综合示例

## 典型的开发调试编译命令

gcc -Wall -Wextra -g -O0 -I ./include main.c helper.c utils.c -L ./lib -lmylib -o myApp

**参数说明：**

- `-Wall -Wextra`：显示所有警告信息

- `-g`：包含调试信息

- `-O0`：关闭优化，便于调试

- `-I ./include`：指定头文件搜索路径

- `-L ./lib`：指定库文件搜索路径

- `-lmylib`：链接自定义库

- `-o my_app`：指定输出文件名

## 实用技巧

1. **增量编译**：修改单个文件时，只需重新编译该文件然后重新链接

2. **调试版本**：使用 `-g -O0` 组合便于调试

3. **发布版本**：使用 `-O2` 或 `-O3` 进行性能优化

4. **严格检查**：始终使用 `-Wall` 帮助发现潜在问题

## 注意事项

- 在 Linux/macOS 下，可执行文件没有扩展名

- 在 Windows 下，可执行文件通常有 `.exe` 扩展名

- 使用 `-Werror` 可以确保代码没有任何警告

- 链接库时注意库文件的顺序，依赖的库应该放在后面
