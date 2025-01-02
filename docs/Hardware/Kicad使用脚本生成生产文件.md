# Kicad使用脚本生成生产文件

作者@Justin

## 安装相关软件

### 安装[Kicad](https://www.kicad.org/)

添加Kicad安装目录下的bin文件夹路径到系统环境变量，如果安装了新的Kicad版本，需把环境变量改为新版本的安装目录。

*如下目录仅供参考，按照实际目录添加*

```
C:\Program Files\KiCad\版本\bin\
```

![image](image/kicad-script-gen-fabs-1.png)
![image](image/kicad-script-gen-fabs-2.png)

***环境变量设置好后需要重启电脑才会生效，切记重启后再执行下面步骤。***

### 安装[Python](https://www.python.org/)依赖库

打开Windows终端，输入如下命令并回车

```
python -m pip install xlsxwriter
```

![image](image/kicad-script-gen-fabs-6.png)

## 执行脚本生成生产文件

打开Windows终端，进入工程仓库目录

建议通过Github Desktop进入终端，这样终端会默认进入仓库目录。

![image](image/kicad-script-gen-fabs-7.png)

执行如下脚本：

```
*python utils\ci.py fab -----用于生成生产文件
*python utils\ci.py rc -----用于检测erc或drc
*python utils\ci.py rc 原理图文件名/PCB文件名 -----检测原理图erc/PCBdrc
*python utils\ci.py fab 文件名/文件夹名 -----生成文件对应板子的生产文件/文件夹内所以板子的生产文件
*python utils\ci.py fab -t tag名-----生成tag对应板子的生产文件
*python utils\ci.py fab 文件名 -n/-ncs-----生成文件对应板子的生产文件,且BOM带不贴的器件
*python utils\ci.py fab 文件名 -j/-jlc-----生成文件对应板子的嘉立创下单生产文件
*python utils\ci.py fab -f 文件名/文件名 文件名 文件名-----生成指定的一个或多个PCB生产文件

查看log信息，确定是否有报错

![image](image/kicad-script-gen-fabs-3.png)

1. 指示生成具体哪个板子
2. 板子版本
3. 板子尺寸和工艺信息
4. 板子层数
5. 指示生成BOM
6. 指示生成Gerber
7. 指示生成坐标文件
8. 指示生成贴片图
