# roboLib使用教程

> @ Justin

## 仓库

[roboLib](https://github.com/Timyerc/roboLib)

## 目录结构说明

```
roboLib/
├── docs/                                   # 文档目录
│   └── Doxyfile
├── examples/                               # 例程目录
│   ├── blink/
│   │   ├── gcc/                            # mm32f0140编译相关文件目录
│   │   │   ├── MM32F0144C6P_FLASH.ld       # 链接文件
│   │   │   ├── Makefile
│   │   │   └── startup_mm32f0140_gcc.s     # 启动文件
│   │   ├── gcc_lcm32f067
│   │   ├── gcc_mm32g0001
│   │   ├── gcc_mm32spin0230
│   │   ├── main.c
│   │   ├── platform.h
│   │   └── target.h
│   └── ...
├── lib/                                    # 依赖库目录
│   ├── CMSIS/                              # ARM Cortex标准头文件目录
│   ├── lcm32f067/
│   ├── mm32f0140/
│   ├── mm32g0001/
│   ├── mm32spin0230/
│   └── ...
├── src/                                    # 源码目录
│   ├── common/                             # 通用代码，如数学计算等快速算法
│   ├── driver/                             # 单片机外设驱动
│   ├── motion/                             # 机器运动相关代码，如IMU、PID等
│   ├── osp/                                # Ovobot serial protocol 串口协议
│   ├── scheduler/                          # 任务调用代码
│   ├── sensors/                            # 常用传感器代码
│   └── ...
├── tests/                                  # 单元测试相关代码
│   ├── AllTest.cpp                         # 单元测试主函数文件
│   ├── Makefile
│   ├── schedulerTest.cpp                   # 任务调度单元测试文件
│   └── ...
├── utils/                                  # 常用工具脚本目录
├── .gitignore
├── LICENSE
└── README.md

```

## 如何使用roboLib创建新项目

推荐从[roboLib-template](https://github.com/Timyerc/roboLib-template)仓库创建新项目，步骤如下：

1. 在 GitHub 上，导航到该模板仓库的主页。

2. 在文件列表上方，点击 Use this template（使用此模板）。

3. 选择 Create a new repository（创建新仓库）。
![use this template](images/use-this-template.png)

> 注意

> 你也可以在 Codespace 中打开此模板，并在之后将你的工作发布为新的仓库。更多信息请参阅 “Creating a codespace from a template”。

4. 在 Owner 下拉菜单中，选择你希望作为该仓库所有者的账号或组织。
![select the account](images/select-account.png)

5. 输入仓库名称，以及可选的描述信息。
![type a name](images/type-name.png)

6. 选择仓库的可见性（公开或私有）。

7. 可选：如果你希望包含模板中所有分支的目录结构与文件，而不仅仅是默认分支，请勾选 Include all branches。

8. 可选：如果你所在的个人账号或组织启用了来自 GitHub Marketplace 的 GitHub 应用，可以在此选择要启用的应用。

9. 点击 Create repository from template（从模板创建仓库）。