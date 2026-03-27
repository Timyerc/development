# C 语言代码风格指南

本文档翻译改编自Betaflight代码风格：[链接](https://betaflight.com/docs/development/CodingStyle)

## 格式化代码风格

### 工具支持

参考[使用VSCode格式化代码](Astyle)

### 缩进

[1TBS](https://en.wikipedia.org/wiki/Indentation_style#Variant:_1TBS_(OTBS)) (based K&R) 4个空格缩进，没有硬件制表符(所有制表符都用空格代替)。

### 大括号

函数在下一行的开始处应该有一个大括号。
```
int function(int x)
{
    body of function
}
```

所有非函数语句块(if、switch、for)的左大括号应该放在同一行最后，接下来的语句从下一行开始。

右大括号只能在代码块中最后一条语句的下一行。

```
if (x is true) {
    we do y
    ...
}
```

```
switch (action) {
case ADD:
    return "add";
case REMOVE:
    return "remove";
case CHANGE:
    return "change";
default:
    return NULL;
}
```

如果后面跟着`else`或者`else if`，则应该放在右大括号的同一行，且再次将左大括号放在同一行上。
```
if (x is true) {
    we do y
    ...
} else {
    we do z
    ...
}
```

在`if`或`else`块只包含一条语句的情况下，不允许省略“不必要的”大括号。这些“单条语句块”可能是未来的一个bug，比如当更多的语句被添加时，你可能会忘了用大括号把语句括起来。

### 空格

在(大多数)关键字之后添加一个空格。值得注意的一些例外情况包括sizeof、typeof、alignof和__attribute__，它们看起来有点像函数(通常与括号一起使用)。因此，在以下关键词后加上空格:
```
if, switch, case, for, do, while
```
但不适用于sizeof, typeof, alignof, 或者 __attribute__ 等关键词。
```
s = sizeof(struct file);
```
当定义指针数据或返回指针类型的函数时，‘*’应靠近数据名称或函数名称，而不是类型名称。例如：
```
char *linux_banner;
memparse(char *ptr, char **retptr);
char *match_strdup(substring_t *s);
```
在大多数二元和三元运算符前后各使用一个空格，比如下面的任何一个:
```
=  +  -  <  >  *  /  %  |  &  ^  <=  >=  ==  !=  ?  :
```
但一元运算符后没有空格:
```
&  *  +  -  ~  !  sizeof  typeof  alignof  __attribute__  defined
```
在递增和递减一元运算符前后都不要有空格
```
++  --
```
 '.' 和 "->" 结构成员操作符前后没有空格

'*' 和 '&', 当用作指针和引用时，它和后面的变量名之间应该没有空格。

## typedef

没有计数或其他终止符元素的枚举应该在最后一个元素后加一个逗号:

```
typedef enum {
    MSP_RESULT_ACK = 1,
    MSP_RESULT_ERROR = -1,
    MSP_RESULT_NO_REPLY = 0,
    MSP_RESULT_CMD_UNKNOWN = -2,
} mspResult_e;
```

这确保了，如果在以后要添加更多的成员，那么在复查中只会显示额外的行，从而使复查更容易。

有计数的枚举应该将计数声明为枚举列表中的最后一项，这样它就会被自动维护，例如:
```
typedef enum {
    PID_CONTROLLER_MW23 = 0,
    PID_CONTROLLER_MWREWRITE,
    PID_CONTROLLER_LUX_FLOAT,
    PID_COUNT
} pidControllerType_e;
```
之后就不用再计算，例如使用PID_CONTROLLER_LUX_FLOAT + 1;

typedef 结构体定义应包含结构体名称，以便可以前向引用该类型，如:
```
typedef struct motorMixer_s {
    float throttle;
...
    float yaw;
} motorMixer_t;
```
motorMixer_s名称是必需的。

## 变量

### 命名

通常情况下，具有描述性的小驼峰（lowerCamelCase）名称是函数名、变量、参数等的理想选择。

变量名应该是名词。

作用域很小的简单临时变量可能会很简短，但这样符合惯例。比如在`for`循环中将”i“作为临时计数器，如`for (int i = 0; i < 4; i++)`。在这种情况下使用“temporaryCounter”不会提高可读性。

### 声明

避免全局变量。

变量应该在使用变量的最小作用域的顶部声明。

应避免变量的重复使用，当它们的作用不相关时，应定义不同的变量。定义后面应有一行空行。

提示：有时你可以创建一个块，即添加大括号，以进一步缩小范围。例如，将变量范围限制为单个`case`分支。

使用有限次数的变量可以在第一次使用的地方声明。它使得代码review更容易（但是如果到处都使用这个变量，这一点就不适用了）。

### 初始化

不要使用`lazy initialisation`。

最好使用显式的`init`函数。

### 数据类型

注意使用的数据类型，不要相信隐式类型转换总是正确的。

避免隐式双精度转换，只使用单浮点参数的函数。

不要直接使用sin() 和 cos()函数，尽量使用近似的效率更高的替代函数。

浮点型的常量需要有`f`后缀，如 1.0f 和 3.1415926f，否则可能会发生双精度转换。

## 函数

### 命名

返回值为布尔型的方法需要命名为一个问句，并且不能改变任何状态。如：`isOkToArm()`

方法需要包含动词或者动宾结构的名字，像deletePage 或者 save。告诉系统‘用(with)’什么去‘做(do)’什么。如：deleteAllPages(pageList)。

非静态函数需要在前面加上它们的类名。如：bareUpdate，而非updateBaro。

一组作用于同一个‘对象’的函数应该共享相同的函数前缀，如：
```
float biQuadFilterApply(...);
void biQuadFilterInit(...);
boolean biQuadIsReady();
```
而不是
```
float applyBiQuadFilter(...);
void newBiQuadLpf(...);
boolean isBiQuadReady();
```

### 参数顺序

参数需要从右向左传递，如在`memcpy(void *dst, const void *src, size\_t size)`。
这个类似赋值操作符（如：dst = src;）

当一组函数作用于同一个‘对象’时那个对象应该作为所有函数第一个参数，如：
```
float biQuadFilterApply(biquad_t *state, float sample);
void biQuadNewLpf(biquad_t *state, float filterCutFreq, uint32_t refreshRate);
```
而不是
```
float biQuadFilterApply(float sample, biquad_t *state);
void biQuadNewLpf(float filterCutFreq, biquad_t *state, uint32_t refreshRate);
```

### 声明

函数没有在包含它的.c文件以外使用的情况下需定义为静态（static）函数（或者 STATIC\_UNIT\_TESTED 这样它们能够在单元测试中使用）。

非静态函数需要在一个唯一的.h文件中有它们的声明。

不要让模块内部的函数对其他模块可见，甚至是类型。预处理宏可以用于定义模块必须与模块对应测试代码共享的一些内部函数等，其他情况都隐藏。

在.h文件中增加如下宏定义：
```
#ifdef MODULENAME_INTERNALS_
… declarations …
#endif
```
在模块的.c文件，和相应测试文件中，在包含.h文件前添加 `#define MODULENAME_INTERNALS_`。

你也可以通过放置在单独的一个.h文件中达到同样的效果。

### 函数实现

保持函数简单并且易分辨。

定义函数时首先应该考虑单元测试。理想的情况下你应该先实现测试用例再实现函数。

- 不要在同一行放置多个语句。
- 不要在同一行放置多个赋值。
- 不要在一个语句中放置多个赋值。

在每次调用时，函数只能从硬件读取一次数据，并且最好只在一处调用。

比如：如果很多地方需要陀螺仪角度或者时间，读取一次并且存储在一个局部变量中。

循环语句尽量使用`for`循环（而不是`do`或`while`）。

禁止使用`continue`和`goto`语言。
函数中有多个`return`和`case`里有多个`break`也是禁止的。
通常这些会降低代码的可读性和可维护性。

在表达式中，括号只能在需要的地方使用，即运算符优先级不能按正确的顺序计算的地方，或者在没有括号的情况下触发编译器警告的地方。这使所有表达式都成为规范形式，并避免了不同开发人员对“易于阅读”的表达式有不同想法的问题。

此规则的一个例外是三元条件运算符

```
pidStabilisationEnabled = (pidControllerState == PID_STABILISATION_ON) ? true : false
```

这里，条件应括在大括号中，以便从左到右阅读时更容易发现三元运算符。

### 函数长度（新增规范）

#### 基本要求

| 类型  | 行数限制 |
| --- | --- |
| 推荐长度 | ≤ 50 行 |
| 最大允许 | ≤ 80 行 |
| 强制拆分 | > 80 行 |

#### 分层限制

| 层级  | 最大行数 |
| --- | --- |
| HAL层 | ≤ 30 行 |
| Driver层 | ≤ 30 行 |
| Control层 | ≤ 40 行 |
| Application层 | ≤ 80 行 |

---

### 函数拆分原则（新增）

满足以下任一条件必须拆分函数：

- 超过 80 行
  
- 包含多个逻辑阶段（如：采样 → 计算 → 控制 → 输出）
  
- 嵌套层级超过 3 层
  
- 存在重复代码
  
- 一屏无法完整显示（IDE 需要滚动）
  
---

### 控制类函数规范（重点新增）

适用于：FOC、PID、运动控制

要求：

- 函数 ≤ 40 行
  
- 必须拆分为步骤函数
  

示例：

```c
void focCurrentLoopRun(void)
{
    clarkeTransform();
    parkTransform();
    currentPiControl();
    svpwmGenerate();
}
```

---

### 状态机函数规范（新增）

```c
void appStateMachineRun(void)
{
    switch (state)
    {
        case stateIdle_e:
            handleIdle();
            break;

        case stateCleaning_e:
            handleCleaning();
            break;
    }
}
```

要求：

- 每个状态独立函数
  
- 禁止在 `case` 内写大量逻辑
  
- 状态机函数 ≤ 80 行
  
---

### 总结（新增）

函数设计核心原则：

> **短小、单一、清晰、可测试**

工程实践标准：

- 推荐 ≤ 50 行
  
- 超过 80 行必须拆分
  
- 控制类函数 ≤ 40 行
  
- 嵌套 ≤ 3 层
  
- 一个函数只做一件事

## Includes

所有文件都必须包含其自己的依赖项，并且不依赖于包含的头文件中的包含项或某些文件需要先包含。

不要包含您不使用的东西。

## 其他

行尾或空行不要有多余空格。

一行保持在 120 列以内，除非超过 120 列会显着提高可读性并且不会隐藏信息。
（少一点是可以接受的。超过 140 则很难在 Github 上阅读，所以永远不要超过。）

最大限度地利用编译时检查，因此通常对待警告要尽可能严格。

不要“向上”调用或者引用。也就是说，不要调用或使用当前层之上的软件层中的任何内容。软件层并不那么明显，但我们可以肯定地说设备驱动程序是底层，因此设备驱动层不应调用或使用其之外的任何内容。

`typedef void handlerFunc(void);` 比 `typedef void (*handlerFuncPtr)(void);` 读起来更容易。

代码应该是球形的。
也就是说，相对于其功能的表面积（公共接口）应该最小化。
这是公共接口应该易于使用的另一种说法，
做一些必要的事情，所有的实现都应该隐藏并且对用户来说不重要。

代码应该在理论上和实践中都有效。
它应该基于合理的数学、物理或计算机科学原理，而不仅仅是启发法。
这对于测试代码也很重要。测试应基于这些原则和现实世界的属性，因此它们不仅仅测试当前的实现。

指南而不是准则：指南并不是完全严格的——只要有充分的理由，它们就可以被打破。
