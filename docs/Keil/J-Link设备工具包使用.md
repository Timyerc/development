# J-Link Device Support Kit (DSK) 文档

***Justin***

*使用ChatGPT翻译自[https://kb.segger.com/J-Link_Device_Support_Kit](https://kb.segger.com/J-Link_Device_Support_Kit)*

## 概述

J-Link设备支持工具包（DSK）用于帮助用户为新的设备添加J-Link支持。通常，支持新设备需要一个闪存加载程序和可能的特殊连接/重置序列。为简化设备支持的创建过程，J-Link DSK提供了 **SEGGER Flash Loader** 和一组示例脚本。

开发过程不需要商业许可证，使用 **SEGGER 的友好许可证** 即可。

---

## 添加新设备

### 创建设备XML文件
要将新设备添加到J-Link软件，需要创建一个包含设备基本信息的XML文件。以下是一个示例：

```xml
<Database>
  <Device>
    <ChipInfo Vendor="SEGGER" Name="SEGGER_Device0" Core="JLINK_CORE_CORTEX_M0"/>
  </Device>
  <Device>
    <ChipInfo Vendor="SEGGER" Name="SEGGER_Device1" Core="JLINK_CORE_CORTEX_M4"/>
  </Device>
</Database>
```

如果设备需要特殊的连接/重置序列，可以通过 `JLinkScriptFile` 指定J-Link脚本文件：

```xml
<Database>
  <Device>
    <ChipInfo Vendor="SEGGER" Name="SEGGER_Device0" Core="JLINK_CORE_CORTEX_M0" JLinkScriptFile="SEGGER/Example.jlinkscript"/>
  </Device>
</Database>
```

### 从现有设备继承
可以使用 `InheritFrom` 属性继承已有设备的属性：

```xml
<Database>
  <Device>
    <ChipInfo Vendor="SEGGER" Name="SEGGER_Device0" Core="JLINK_CORE_CORTEX_M0" JLinkScriptFile="SEGGER/Example.jlinkscript"/>
  </Device>
  <Device InheritFrom="SEGGER_Device0">
    <ChipInfo Name="SEGGER_Device1"/>
  </Device>
</Database>
```

---

## 添加闪存加载程序（Flash Loader）

### 配置示例
可以为设备配置一个或多个闪存加载程序。以下是一个设备XML文件的示例，显示如何添加多个闪存加载程序：

```xml
<Database>
  <Device>
    <ChipInfo Vendor="SEGGER" Name="SEGGER_Device0" Core="JLINK_CORE_CORTEX_M4" />
    <!-- 内部代码闪存 -->
    <FlashBankInfo Name="Internal code flash" BaseAddr="0x08000000" AlwaysPresent="1" >
      <LoaderInfo Name="Default" MaxSize="0x80000" Loader="Flashloader_Device0_InternalCodeFlash.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
    </FlashBankInfo>
    <!-- 外部QSPI闪存 -->
    <FlashBankInfo Name="External QSPI flash" BaseAddr="0x60000000" AlwaysPresent="1" >
      <LoaderInfo Name="PinConfig1" MaxSize="0x1000000" Loader="Flashloader_Device0_ExternalQSPIFlash_PinConfig1.elf" LoaderType="FLASH_ALGO_TYPE_OPEN" />
    </FlashBankInfo>
  </Device>
</Database>
```

### 设备与加载程序的关联
以下是J-Link DSK支持的几种常见关联方式：
- 1:0:0 = 1个设备，0个闪存，0个闪存加载程序
- 1:1:1 = 1个设备，1个闪存，1个闪存加载程序
- 1:m:n = 1个设备，n个闪存，m个加载程序

---

## JLinkDevices 文件夹

### 文件夹位置
为了让J-Link软件识别新设备，XML文件需要放置在J-Link的中央文件夹中。文件夹路径如下：

| 操作系统   | 路径                                                    |
| ---------- | ------------------------------------------------------- |
| Windows    | C:\Users\<USER>\AppData\Roaming\SEGGER\JLinkDevices     |
| Linux      | $HOME/.config/SEGGER/JLinkDevices                       |
| macOS      | $HOME/Library/Application Support/SEGGER/JLinkDevices  |

### 示例文件夹结构
```plaintext
JLinkDevices
    +---Vendor1
    |   +---DevFamily1
    |   |       Devices.xml
    |
    +---Vendor2
        +---DevFamily1
            Devices.xml
```

---

## XML 文件结构

### 主要标签说明

- `<Database>`: XML文件的顶级标签。
- `<Device>`: 描述一个新设备的标签。
- `<ChipInfo>`: 描述设备的核心信息，如核心类型等。
- `<FlashBankInfo>`: 描述设备的闪存信息。

### XML 标签示例

#### `<ChipInfo>` 示例

```xml
<ChipInfo Vendor="SEGGER" Name="SEGGER_Device0" Core="JLINK_CORE_CORTEX_M4" WorkRAMAddr="0x20000000" WorkRAMSize="0x8000"/>
```

#### `<FlashBankInfo>` 示例

```xml
<FlashBankInfo Name="Internal code flash" BaseAddr="0x08000000" AlwaysPresent="1">
  <LoaderInfo Name="Default" MaxSize="0x80000" Loader="Flashloader_Device0_InternalCodeFlash.elf" LoaderType="FLASH_ALGO_TYPE_OPEN"/>
</FlashBankInfo>
```

---

## 结论

通过J-Link DSK，用户可以轻松地为新的设备创建J-Link支持，添加闪存加载程序，并通过XML文件将新设备添加到J-Link工具链中。该工具包还提供了支持多种设备的脚本和模板，极大简化了设备支持的流程。

### DSK获取
若要获得DSK，可以联系 SEGGER 官方：[info@segger.com](mailto:info@segger.com)。此外，也可以使用CMSIS加载程序作为替代方案。