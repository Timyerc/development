# VS Code通过J-LINK仿真自定义MCU

## 1、获取芯片下载算法.FLM

![image](images\图片1.png)

## 2、在JLinkDevices.xml文件当中添加芯片描述和.FLM的相对路径

![image](images\图片2.png)

```
  <!--          -->
  <!-- Linsynix  -->
  <!--          -->
  <Device>
    <ChipInfo Vendor="Linsynix" Name="LCM32F067" WorkRAMAddr="0x20000000" WorkRAMSize="0x00002800" Core="JLINK_CORE_CORTEX_M0"/>
    <FlashBankInfo Name="Flash Block" BaseAddr="0x8000000" MaxSize="0x10000" Loader="Devices\Linsynix\Flash\LCM32F067_FLASH.FLM" LoaderType="FLASH_ALGO_TYPE_OPEN" AlwaysPresent="1"/>
  </Device>
```

<!-- Linsynix  -->

<!--          -->

<Device>
 <ChipInfo Vendor="Linsynix" Name="LCM32F067" WorkRAMAddr="0x20000000" WorkRAMSize="0x00002800" Core="JLINK_CORE_CORTEX_M0"/>
 <FlashBankInfo Name="Flash Block" BaseAddr="0x8000000" MaxSize="0x10000" Loader="Devices\Linsynix\Flash\LCM32F067_FLASH.FLM" LoaderType="FLASH_ALGO_TYPE_OPEN" AlwaysPresent="1"/>
 </Device>

## 3、使用J-FLASH下载测试

![image](images\图片3.png)

![image](images\图片4.png)

![image](images\图片5.png)

![image](images\图片6.png)

# 4、VS Code添加，仿真测试

![](images\图片7.png)

```
        {
            "name": "Test",
            "cwd": "${workspaceFolder}",
            "executable": ".\\Project\\Templates\\gcc\\build\\gcc_test.elf",  
            "request": "launch",
            "type": "cortex-debug",
            "runToEntryPoint": "main",
            "servertype": "jlink",
            "interface": "swd",
            "device": "LCM32F067",
            "toolchainPrefix": "arm-none-eabi",
            "serverpath": "C:\\Program Files\\SEGGER\\JLink\\JLinkGDBServerCL.exe",
            "svdFile": "C:\\Keil_v5\\ARM\\PACK\\Keil\\LCM32F0xx_DFP\\0.4.71\\SVD\\LCM32F067.svd",
            "liveWatch": {
                "enabled": true,
                "samplesPerSecond": 4
            }
        }
```

## 注意事项：

1、此方法适用于V7.64版本及更早的JLINK版本

2、芯片下载算法.FLM，在安装keil后，并添加芯片.pack之后，可在keil的安装路径下找到

3、在按照上述方法添加MCU后，如果仍然无法下载程序，可尝试更换.FLM下载算法
