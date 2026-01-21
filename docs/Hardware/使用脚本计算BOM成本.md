# 使用脚本计算BOM成本

作者@Justin

### 安装[Python](https://www.python.org/)依赖库

打开Windows终端，输入如下命令并回车

```
python -m pip install xlsxwriter
python -m pip install pymupdf
python -m pip install --upgrade pywin32
```

### 在工程根目录执行如下脚本查看命令帮助

```
> python .\utils\ci.py -h
usage: ci.py [-h] {rc,fab,pdf} ...

PCB设计CI脚本

positional arguments:
  {rc,fab,pdf}  子命令
    rc          检测原理图ERC, 或者PCB DRC
    fab         生成生产文件, 如BOM, GERBER, 坐标, 贴片图等
    pdf         生成带水印的原理图、PCB和BOM的PDF文件

options:
  -h, --help    show this help message and exit
```

ci.py有三个子命令，分别是rc, fab和pdf。 rc用于检测原理图ERC, 或者PCB DRC；fab用于生成生产文件, 如BOM, GERBER, 坐标, 贴片图等；pdf用于生成带水印的原理图、PCB和BOM的PDF文件。

每个子命令的使用方法可以查看子命令的帮助：

```
python utils\ci.py 子命令 -h
```

例如：

```
> python .\utils\ci.py fab -h
usage: ci.py fab [-h] [-t TAG] [-n] [-j] [-f FILE_LIST [FILE_LIST ...]] [folder]

positional arguments:
  folder                生成对应文件夹下或所有子文件夹下的PCB的生产文件

options:
  -h, --help            show this help message and exit
  -t TAG, --tag TAG     生成tag对应的PCB生产文件, 不用输入文件或者文件夹名
  -n, --ncs             BOM输出不贴的元器件
  -j, --jlc             生成嘉立创贴片相关资料
  -f FILE_LIST [FILE_LIST ...], --file_list FILE_LIST [FILE_LIST ...]
                        一个或者多个PCB文件
```
