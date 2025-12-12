# 串口MSP协议

## 客户端包 Client Packages

\<preamble>,\<option>,\<device>,\<command>,\<size>,\<data>,\<crc>
| 项 | 描述 | 注释 |
| ----------- | ----------- | ----------- |
| preamble | 包头 | 总是为 0xFF |
| option | 选项 | 见下文 |
| device | 设备码 | 此包适用于设备 |
| command | 包命令 | 见下文 |
| size | 数据所占字节数 | |
| data | 数据 | 命令附带的可选数据 |
| crc | crc 校验和 | \<device>,\<command>,\<size>的异或运算，将每个数据字节归为零和 |

\<option> 位描述

缺省: 0xFF
| 位 7-1 | 位 0 |
| -------- | ----- |
| Reserved | Answer |
- Answer – 当设置为1时，执行该命令并发送回复。0时，行动但不回应。

## 响应包 Responce Packages

\<preamble>,\<option>,\<command>,\<size>,\<data>,\<crc>
| 项 | 描述 | 注释 |
| ----------- | ----------- | ----------- |
| preamble | 包头 | 总是为 0xFF |
| option | 选项 | 当这是一个确认时设置为0xFF，当这是一个异步消息时设置为0xFE |
| command | 包命令 | 见下文 |
| size | 数据所占字节数 | |
| data | 数据 | 命令附带的可选数据 |
| crc | crc 校验和 | \<device>,\<command>,\<size>的异或运算，将每个数据字节归为零和 |

## 设备码 Device Code

| 值 | 名称 |
| ----- | ---- |
| 0x00 | DCODE_CORE |
| 0x03 | DCODE_CLEANROBOT (VER_HW) 擦窗机 |
| 0x04 | DCODE_SOLAR_CLEANBOT (VER_HW) 光伏清洁机器人 |
| 0x05 | DCODE_BLDC (VER_HW) 无刷电机 |

## 核心命令 Core Command

| 值 | 名称 |
| ----- | ---- |
| 0x01 | CMD_PING |
| 0x02 | CMD_VERSION |
| 0x03 | CMD_GET_POWER_STATE |
| 0x04 | CMD_SET_POWER_NOTIFY |
| 0x05 | CMD_ENTER_BOOTLOADER (CMD_BUILD_INFO) |
| 0x06 | CMD_BINARY_INFO |
| 0x07 | CMD_BINARY_OFFSET |
| 0x08 | CMD_BINARY_TRANS |
| 0x09 | CMD_BINARY_TRANS_OVER |

## 光伏

### 光伏命令 Command

| 值 | 名称 | 描述 |
| ----- | ---- | ---- |
| 97 | CMD_RANGEFINDER | 获取超声波距离 |
| 98 | CMD_SET_HEARTBEAT | 设置心跳值(s)，默认1s发送一次心跳包 |
| 99 | CMD_SET_ROTATE | 设置旋转角度值 |
| 100 | CMD_SET_SPEED | 设置速度值 |
| 101 | CMD_SET_LINE_STATUS | 设置栅线数据 |
| 102 | CMD_RAW_IMU | 获取陀螺仪加速度原始值 |
| 103 | CMD_GET_LINE_STATUS | 获取栅线数据 |
| 104 | CMD_MOTOR_SPEED | 获取速度 |
| 105 | CMD_CROSS_BATT | 获取跨电池片数量 |
| 108 | CMD_ATTITUDE | 获取姿态角 |
| 109 | CMD_SYSTEM | 获取系统任务信息 |
| 110 | CMD_GET_REVEIVER | 获取遥控器通道数据 |
| 111 | CMD_FRAMERATE | 设置视觉处理帧率 |
| 130 | CMD_BATTERY_STATE | 获取电池状态 |
| 132 | CMD_GET_TEMP | 获取电机温度 |
| 133 | CMD_GET_MOVE_STATUS | 获取运动状态 |
| 134 | CMD_SET_CELL_INFO | 设置电池片尺寸信息 |

### 光伏命令细节 Command Detial

#### CMD_RANGEFINDER 97

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 97    | 0x00 |

**接收：**

| command | size | left front | right front | left back | right back | sample rate |
| ------- | ---- | -----------| ----------- | ----------| ---------- | ----------  |
| 97    | 0x0A | 16 bit unsigned  | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

#### CMD_SET_HEARTBEAT 98

**发送：**

| device | command | size | rotate value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 98    | 0x02 | 16 bit signed   |

**接收：**

Simple response

#### CMD_SET_ROTATE 99

**发送：**

| device | command | size | rotate value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 99    | 0x02 | 16 bit signed   |

**接收：**

Simple response

#### CMD_SET_SPEED 100

**发送：**

| device | command | size | speed value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 100    | 0x02 | 16 bit signed   |

**接收：**

Simple response

#### CMD_SET_LINE_STATUS 101

**发送：**

| device | command | size | busbar distance | busbar angle  | gap distance  | gap angle     | state          |
| ------ | ------- | ---- | ----------------| ------------- | ------------  | ------------- | -------------- |
| 0x03   | 101    | 0x08 | 16 bit signed   | 16 bit signed | 16 bit signed | 16 bit signed | 8 bit unsigned |

| state     | value |
| --------- | ----- |
| 正常       | 0x00  |
| 竖直有数据 | 0x01  |
| 水平有数据 | 0x02  |
| 异常       | 0x03  |

**接收：**

Simple response

#### CMD_MOTOR_SPEED 104

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 104    | 0x00 |

**接收：**

| command | size | left raw speed | right raw speed | average speed | move distance |
| ------- | ---- | ---------------| --------------- | --------------| ------------- |
| 104    | 0x0C | 32 bit signed  | 32 bit signed   | 16 bit signed | 16 bit signed |

#### CMD_ATTITUDE 108

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 108    | 0x00 |

**接收：**

| command | size | yaw | pitch | roll | merge yaw |
| ------- | ---- | ---------------| --------------- | --------------| ------------- |
| 108    | 0x08 | 16 bit signed  | 16 bit signed   | 16 bit signed | 16 bit signed |

#### CMD_GET_REVEIVER 110

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 110    | 0x00 |

**接收：**

| command | size | thr | yaw | arm | mode | sucker | brush | failsafe |
| ------- | ---- | --- | --- | ----| ---- | -------| ----- | -------- |
| 110    | 0x0E | 16 bit unsigned  | 16 bit unsigned   | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

#### CMD_FRAMERATE 111

**发送：**

| device | command | size | rotate value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 111    | 0x02 | 16 bit signed   |

**接收：**

Simple response

#### CMD_BATTERY_STATE 130

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 130    | 0x00 |

**接收：**

| command | size | battery voltage | battery state |
| ------- | ---- | ----------------| -------------- |
| 130    | 0x03 | 16 bit unsigned | 8 bit unsigned |

电池电压为0.01V级

| battery state | value |
| ------------- | ----- |
| BATTERY_OK    | 0x00 |
| BATTERY_WARNING | 0x01 |
| BATTERY_CRITICAL | 0x02 |
| BATTERY_NOT_PRESENT | 0x03 |

#### CMD_GET_TEMP 132

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 132    | 0x00 |

**接收：**

| command | size | temperature |
| ------- | ---- | ------------|
| 132    | 0x01 | 16 bit signed |

温度以10℃为一级

#### CMD_GET_MOVE_STATUS 133

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 133    | 0x00 |

**接收：**

| command | size | move mode | path state |
| ------- | ---- | ------------| -------- |
| 133    | 0x02 | 8 bit unsigned | 8 bit unsigned |

#### CMD_SET_CELL_INFO 134

**发送：**

| device | command | size | cell width | cell type | run dir  |
| ------ | ------- | ---- | -----------| --------- | -------- |
| 0x03   | 134    | 0x04 | 16 bit unsigned | 8 bit unsigned | 8 bit unsigned |

cell width: 电池片短边宽度

cell type: 0 半片；1 全片

run dir: 0 沿着长边运动；1 沿着短边运动

**接收：**

Simple response

## 擦窗机

### 擦窗机命令 Command

| 值 | 指令名称 | 描述 | 圆形专用 | 方形专用 |
| ----- | ---- | ---- | ---- | ---- |
| 读取 |
| 102 | MSP_RAW_IMU | 陀螺仪加速度原始值 |
| 103 | MSP_DATA_POINT | 测试数据点，测试专用，数据格式可以修改 |
| 104 | MSP_GYRO_DETECT | 陀螺仪撞边判断 | ✓ |
| 105 | MSP_EDGE_BOTTOM_DETECT | 底边检测 | ✓ |
| 106 | MSP_MACHINE_STATE | 机器工作状态，水平or竖直工作状态 |
| 107 | MSP_THRESHOLD | 获取边轮马达电流参数 | ✓ |
| 108 | MSP_ATTITUDE | 机器姿态角 |
| 110 | MSP_ANALOG | AD数据 |
| 111 | MSP_ADAPTER | 适配器电压 |
| 112 | MSP_FOURCORNER | 悬空开关 | | ✓ |
| 113 | MSP_BARO_DIFF | 无边气压检测 |
| 114 | MSP_WATER_BOX | 水量检测 |
| 115 | MSP_WIFI_RSSI | 获取WIFI产测结果 |
| 117 | MSP_FAST_CURRENT | 直行电流判断 | | ✓ |
| 118 | MSP_Z_TURN_FAST_CURRENT | Z字水平换行电流 | | ✓ |
| 120 | MSP_SYSTICK | 系统周期 |
| 121 | MSP_ORIGIN_ARGS | 回原点数据 | ✓ |
| 123 | MSP_MOTOR_CURRENT | 边轮马达实际电流值 * 100 |
| 130到136指令只有在使用USE_UART_CONFIG功能有效 |
| 130 | MSP_GET_PWMVALUE | 获取风机占比 |
| 131 | MSP_GET_USE_FAN_LEVEL_DYNAMIC_COMP | 获取风机最大最小占空比 |
| 132 | MSP_GET_USE_FAN_OUTPUT_PID | 获取风机恒压占比 |
| 133 | MSP_GET_MOTOR_VALUE | 获取马达最小占空比 |
| 134 | MSP_GET_BOUNDLESS | 获取无边判断阈值 |
| 135 | MSP_GET_SPRAY_VALUE | 获取喷水功能参数 |
| 136 | MSP_GET_GYRO_THRESHOLD | 获取陀螺仪阈值 |
| 140 | MSP_GET_FAN_PID_PARAM | 获取风机PID参数 |
| 142 | MSP_GET_FAN_PID_RESULT | 获取风机目标负压和实时负压 |
| 146 | MSP_GET_BATTERY_CHARGE_PARAM | 获取分立充电参数 |
| 148 | MSP_GET_AUTO_TEST_RESULT | 获取自动化测试结果 |
| 151 | MSP_GET_REMOTE | 获取遥控器指令 |
| 设置 |
| 116 | MSP_WIFI_TEST | WIFI开始产测 |
| 141 | MSP_SET_FAN_PID_PARAM | 设置风机PID参数 |
| 147 | MSP_SET_AUTO_TEST_RESULT | 设置自动化测试结果 |
| 205 | MSP_ACC_CALIBRATION | 加速度校准 |
| 208 | MSP_PLAY_VOICE | 测试语音 |
| 209 | MSP_SET_SPRAY | 控制喷水 |
| 211 | MSP_SET_FAN | 控制风机 |
| 214 | MSP_SET_MOTOR | 控制马达 |
| 215 | MSP_SET_TRIGGER | 发送触发信号，发送和接收2个16位数据 |
| 216 | MSP_SET_TRIGGER_2 | 发送触发信号，发送和接收4个16位数据 |
| 220 | MSP_SET_PWMVALUE | 设置风机占比 |
| 221 | MSP_SET_USE_FAN_LEVEL_DYNAMIC_COMP | 设置风机最大最小占比 |
| 222 | MSP_SET_USE_FAN_OUTPUT_PID | 设置风机恒压占比 |
| 223 | MSP_SET_MOTOR_VALUE | 设置马达最小占比 |
| 224 | MSP_SET_BOUNDLESS | 设置无边电流 |
| 225 | MSP_SET_SPRAY_VALUE | 设置喷水功能 |
| 226 | MSP_SET_GYRO_THRESHOLD | 设置陀螺仪阈值 |

### 擦窗机命令细节

**读取**

#### MSP_RAW_IMU 102

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 102    | 0x00 |

**接收：**

| device | command | size | gyro x | gyro y | gyro z | acce x | acce y | acce z |
| ------ | ------- | ---- | -------| -------| -------| -------| -------| -------|
| 0x03   | 102    | 0x0C | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

陀螺仪和加速度原始值，数值范围 -32767 ~ 32767

#### MSP_DATA_POINT 103

> 默认两个16位数据，根据调试需要可以修改数据长度

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 103    | 0x00 |

**接收：**

| device | command | size | data0 | data1 | data2 |
| ------ | ------- | ---- | -----------| -----------| -----------|
| 0x03   | 103    | 0x06 | 16 bit signed | 16 bit signed | 16 bit signed |

#### MSP_GYRO_DETECT 104

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 104    | 0x00 |

**接收：**

| device | command | size | gyro diff | sample time |
| ------ | ------- | ---- | -----------| -----------|
| 0x03   | 104    | 0x04 | 16 bit signed | 16 bit unsigned |

gyro diff: 陀螺仪变化值，用于观察机器在运行时陀螺仪的变化值，诊断机器误判原因

sample time: 程序运行主循环的周期，单位0.1ms。

#### MSP_EDGE_BOTTOM_DETECT 105

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 105    | 0x00 |

**接收：**

| device | command | size | gyro diff 1 | gyro diff 2 | gyro z |
| ------ | ------- | ---- | -----------| -----------| -----------|
| 0x03   | 105    | 0x06 | 16 bit signed | 16 bit signed | 16 bit signed |

gyro diff 1: 陀螺仪变化值1

gyro diff 2: 陀螺仪变化值2

gyro z: 陀螺仪z轴原始值

#### MSP_MACHINE_STATE 106

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 106    | 0x00 |

**接收：**

| device | command | size | state | acce x | acce y | acce z |
| ------ | ------- | ---- | -----------| -----------| -----------| -----------|
| 0x03   | 106    | 0x08 | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

state: 机器状态；0，未知；1，水平工作状态；2，竖直工作状态；3，水平倒吸工作状态

acce x: x轴加速度平均值

acce y: x轴加速度平均值

acce z: z轴加速度平均值

#### MSP_THRESHOLD 107

> 在使用`USE_FAN_LEVEL_DYNAMIC_COMP`功能时有效

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 107    | 0x00 |

**接收：**

| device | command | size | thres | min cnt | max cnt | pressure |
| ------ | ------- | ---- | -----------| -----------| -----------| -----------|
| 0x03   | 107    | 0x08 | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

thres: 边轮ADC原始平均值

min cnt: 平均值小于阈值次数

max cnt: 平均值大于阈值次数

pressure: 使用闭环吸力时，此值为目标负压值；否则为风机pwm值

#### MSP_ATTITUDE 108

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 108    | 0x00 |

**接收：**

| device | command | size | euler x | euler y | euler z |
| ------ | ------- | ---- | -----------| -----------| -----------|
| 0x03   | 108    | 0x06 | 16 bit signed | 16 bit signed | 16 bit signed |

euler x: x轴欧拉角度

euler y: y轴欧拉角度

euler z: z轴欧拉角度

#### MSP_ANALOG 110

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 110    | 0x00 |

**接收：**

| device | command | size | adc left | adc right | adc fan |
| ------ | ------- | ---- | -----------| -----------| -----------|
| 0x03   | 110    | 0x06 | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

adc left: 左边轮电流原始ADC值

adc right: 右边轮电流原始ADC值

adc fan: 风机电流原始ADC值

#### MSP_ADAPTER 111

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 111    | 0x00 |

**接收：**

| device | command | size | adapter voltage | battery voltage |
| ------ | ------- | ---- | -----------| -----------|
| 0x03   | 111    | 0x04 | 16 bit unsigned | 16 bit unsigned |

adapter voltage：适配器电压 x 10

battery voltage：电池电压 x 10

#### MSP_FOURCORNER  112

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 112     | 0x00 |

**接收：**

| device | command | size | state |
| ------ | ------- | ---- | -----------|
| 0x03   | 112    | 0x04 | 8 bit unsigned |

state：4脚浮空开关状态，位0左上角，位1右上角，位2左下角，位3右下角

#### MSP_BARO_DIFF 113

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 113     | 0x00 |

**接收：**

| device | command | size | baro diff | baro origin | baro standard |
| ------ | ------- | ---- | -----------| -----------| -----------|
| 0x03   | 113     | 0x0A | 16 bit signed | 32 bit unsigned | 32 bit unsigned |

baro diff: 机器运行过程中气压差值

baro origin：气压实时值

baro standard：机器启动时采集到的标准大气压值

#### MSP_WATER_BOX 114

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 114    | 0x00 |

**接收：**

| device | command | size | water status real | water adc | water status filter |
| ------ | ------- | ---- | -----------| -----------| -----------|
| 0x03   | 114    | 0x05 | 8 bit unsigned | 16 bit unsigned | 16 bit unsigned |

water status real：水量状态实时值

water adc：水量ADC值

water status filter：水量状态滤波值

#### MSP_WIFI_RSSI 115

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 115    | 0x00 |

**接收：**

| device | command | size | rssi |
| ------ | ------- | ---- | -----------|
| 0x03   | 115    | 0x01 | 8 bit unsigned |

rssi：wifi信号强度，0 ~ 100

#### MSP_FAST_CURRENT 117

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 117    | 0x00 |

**接收：**

| device | command | size | detect cnt | diff |
| ------ | ------- | ---- | -----------| -----------|
| 0x03   | 117    | 0x03 | 8 bit unsigned | 16 bit signed |

detect cnt：检测有效次数

diff：边轮电流ADC差值

#### MSP_Z_TURN_FAST_CURRENT 118

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 118    | 0x00 |

**接收：**

| device | command | size | single diff |
| ------ | ------- | ---- | ----------- |
| 0x03   | 118     | 0x02 | 16 bit signed |

single diff：单边电流与均值差值

#### MSP_SYSTICK 120

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 120    | 0x00 |

**接收：**

| device | command | size | sample time  |
| ------ | ------- | ---- | ----------- |
| 0x03   | 120     | 0x02 | 16 bit signed |

sample time: 程序运行主循环的周期，单位0.1ms。

#### MSP_ORIGIN_ARGS 121

> 有回原点功能有效

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 121     | 0x00 |

**接收：**

| device | command | size | y2top steps | x2right steps | x total steps | x run steps |
| ------ | ------- | ---- | ----------- | ----------- | ----------- | ----------- |
| 0x03   | 121     | 0x08 | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

y2top steps：垂直到顶部的步数

x2right steps：水平到右边的步数

x total steps：水平总步数

x run steps：水平已走步数

#### MSP_MOTOR_CURRENT 123

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 123     | 0x00 |

**接收：**

| device | command | size | current left | current right |
| ------ | ------- | ---- | ----------- | ----------- |
| 0x03   | 123     | 0x04 | 16 bit unsigned | 16 bit unsigned |

current left：左轮电流值

current right：右轮电流值

#### MSP_GET_PWMVALUE 130

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 130    | 0x00 |

**接收：**

| device | command | size | pwmValue |
| ------ | ------- | ---- | -----------|
| 0x03   | 130    | 0x01 | 8 bit unsigned |

pwmValue：风机PWM占空比输出

#### MSP_GET_USE_FAN_LEVEL_DYNAMIC_COMP 131

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 131    | 0x00 |

**接收：**

| device | command | size | pwmvalueMax | pwmvalueMin|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 131    | 0x02 | 8 bit unsigned |8 bit unsigned |

pwmvalueMax：动态吸力最大风机PWM占空比

pwmvalueMin：动态吸力最小风机PWM占空比

#### MSP_GET_USE_FAN_OUTPUT_PID 132

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 132    | 0x00 |

**接收：**

| device | command | size | fanPwmvalueAtIdle | fanPwmvalueMin | fanPwmvalueMax |defaultPressure | maxPressure | minPressure |
| ------ | ------- | ---- | -----------|-----------|------- | ---- | -----------|-----------|
| 0x03   | 132    | 0x09 | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

fanPwmvalueAtIdle：待机状态风机PWM占空比

fanPwmvalueMin：最小风机PWM占空比

fanPwmvalueMax：最大风机PWM占空比

defaultPressure：默认目标吸力

maxPressure：最大吸力

minPressure：最小吸力

#### MSP_GET_MOTOR_VALUE 133

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 133    | 0x00 |

**接收：**

| device | command | size | minPwmValue | upMinPwmValue|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 133    | 0x02 | 8 bit unsigned |8 bit unsigned |

minPwmValue：水平运动边轮最小PWM占空比

upMinPwmValue：向上运动边轮最小PWM占空比

#### MSP_GET_BOUNDLESS 134

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 134    | 0x00 |

**接收：**

| device | command | size | fanThres | fanUpThres | hangCnt |
| ------ | ------- | ---- | -----------|-----------| -----------|
| 0x03   | 134    | 0x03 | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned |

fanThres：水平运动无边检测阈值

fanUpThres：竖直运动无边检测阈值

hangCnt：无边检测次数

#### MSP_GET_SPRAY_VALUE 135

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 135    | 0x00 |

**接收：**

| device | command | size | waterPump | duration | startAngle | moveCnt |
| ------ | ------- | ---- | -----------|-----------| -----------|-----------|
| 0x03   | 135    | 0x05 | 8 bit unsigned | 16 bit unsigned | 8 bit unsigned | 8 bit unsigned |

waterPump：喷水功能开关状态，0无喷水功能，1有喷水功能

duration：喷水单次持续时间

startAngle：喷水开启角度

moveCnt：开启喷水步数间隔

#### MSP_GET_GYRO_THRESHOLD 136

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 136    | 0x00 |

**接收：**

| device | command | size | gyroDiff | gyroThreshold | gyroUpDiff | gyroUpThreshold |
| ------ | ------- | ---- | -------- | ------------- | ---------- | --------------- |
| 0x03   | 136     | 0x04 | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned |

gyroDiff：水平运动判断撞边陀螺仪变化值

gyroThreshold：水平运动判断撞边陀螺仪绝对值

gyroUpDiff：竖直运动判断撞边陀螺仪变化值

gyroUpThreshold：竖直运动判断撞边陀螺仪绝对值

#### MSP_GET_FAN_PID_PARAM 140

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 140    | 0x00 |

**接收：**

| device | command | size | kp | ki | kd |
| ------ | ------- | ---- | -------- | ------------- | ---------- |
| 0x03   | 140     | 0x0C | 32 bit unsigned | 32 bit unsigned | 32 bit unsigned |

kp：比例参数 * 1000，范围 0.01 ~ 1.0

ki：积分参数 * 1000，0.001 ~ 0.01

kd：微分参数 * 1000，0.0001 ~ 0.001

#### MSP_GET_FAN_PID_RESULT 142

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 142    | 0x00 |

**接收：**

| device | command | size | target pressure | real pressure |
| ------ | ------- | ---- | -------- | ------------- |
| 0x03   | 142     | 0x08 | 32 bit unsigned | 32 bit unsigned |

target pressure：目标吸力

real pressure：实时吸力

#### MSP_GET_BATTERY_CHARGE_PARAM 146

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 146    | 0x00 |

**接收：**

| device | command | size | charge state | battery voltage | charge current | battery cell |
| ------ | ------- | ---- | -------- | ------------- |
| 0x03   | 146     | 0x05 | 8 bit unsigned | 8 bit unsigned | 16 bit unsigned | 8 bit unsigned |

charge state：充电状态，0未知，1慢充，2快充，3恒压，

battery voltage：电池电压 * 10

charge current：充电电流 * 1000

bettery cell：电池节数

#### MSP_GET_AUTO_TEST_RESULT 148

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 148    | 0x00 |

**接收：**

| device | command | size | completed |
| ------ | ------- | ---- | -------- |
| 0x03   | 148     | 0x01 | 8 bit unsigned |

completed：1为完成，0为未完成

#### MSP_GET_REMOTE 151

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 151    | 0x00 |

**接收：**

| device | command | size | remote code |
| ------ | ------- | ---- | -------- |
| 0x03   | 151     | 0x08 | 8 bit unsigned |

remote code：遥控器码

**设置**

#### MSP_SET_FAN_PID_PARAM 141

**发送：**

| device | command | size | kp | ki | kd |
| ------ | ------- | ---- | ---- | ---- | ---- |
| 0x03   | 141    | 0x0C | 32 bit unsigned | 32 bit unsigned | 32 bit unsigned |

**接收：**

Simple response

#### MSP_SET_AUTO_TEST_RESULT 147

**发送：**

| device | command | size | completed |
| ------ | ------- | ---- | ---- |
| 0x03   | 147    | 0x0C | 8 bit unsigned |

completed：完成写1，否则写0

**接收：**

Simple response

#### MSP_WIFI_TEST 116

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 116    | 0x00 |

**接收：**

Simple response

#### MSP_ACC_CALIBRATION 205

**发送**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 205     | 0x00 |

**接收**

Simple response

#### MSP_PLAY_VOICE 208

**发送**

| device | command | size | voice index |
| ------ | ------- | ---- | -----------|
| 0x03   | 208     | 0x01 | 8 bit unsigned |

voice index：语音索引

**接收**

Simple response

#### MSP_SET_SPRAY 209

**发送**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 209     | 0x00 |

**接收**

Simple response

#### MSP_SET_FAN 211

**发送**

| device | command | size | pwm value |
| ------ | ------- | ---- | -----------|
| 0x03   | 211    | 0x01 | 8 bit unsigned |

pwm value：风机占空比，0~100

**接收**

Simple response

#### MSP_SET_MOTOR 214

**发送**

| device | command | size | dir |
| ------ | ------- | ---- | -----------|
| 0x03   | 214    | 0x01 | 8 bit unsigned |

dir：设置边轮马达运动方向，0停转，1正转，2反转

**接收**

Simple response

#### MSP_SET_TRIGGER 215

**发送**

| device | command | size | send 0 | send 1 |
| ------ | ------- | ---- | ------ | ------ |
| 0x03   | 215     | 0x04 | 16 bit signed | 16 bit signed |

**接收**

| device | command | size | recv 0 | recv 1 |
| ------ | ------- | ---- | -------- | ------------- |
| 0x03   | 136     | 0x04 | 16 bit signed | 16 bit signed |

#### MSP_SET_TRIGGER_2 216

**发送**

| device | command | size | send 0 | send 1 | send 2 | send 3 |
| ------ | ------- | ---- | ------ | ------ | ------ | ------ |
| 0x03   | 215     | 0x04 | 16 bit signed | 16 bit signed | 16 bit signed | 16 bit signed |

**接收**

| device | command | size | recv 0 | recv 1 | recv 2 | recv 3 |
| ------ | ------- | ---- | -------- | -------- | -------- | -------- |
| 0x03   | 136     | 0x04 | 16 bit signed | 16 bit signed | 16 bit signed | 16 bit signed |

#### MSP_SET_PWMVALUE 220

**发送**

| device | command | size | pwmValue |
| ------ | ------- | ---- | -----------|
| 0x03   | 220    | 0x01 | 8 bit unsigned |

pwmValue：风机占空比，0~100

**接收**

Simple response

#### MSP_SET_USE_FAN_LEVEL_DYNAMIC_COMP 221

**发送：**

| device | command | size | pwmvalueMax | pwmvalueMin|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 221    | 0x02 | 8 bit unsigned |8 bit unsigned |

**接收：**

Simple response

#### MSP_SET_USE_FAN_OUTPUT_PID 222

**发送：**

| device | command | size | fanPwmvalueAtIdle | fanPwmvalueMin | fanPwmvalueMax | defaultTargetFanPwmvalue | maxTargetFanPwmvalue | minTargetFanPwmvalue |
| ------ | ------- | ---- | -----------|-----------|------- | ---- | -----------|-----------|
| 0x03   | 222    | 0x09 | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

**接收：**

Simple response

#### MSP_SET_MOTOR_VALUE 223

**发送：**

| device | command | size | minPwmValue | upMinPwmValue |
| ------ | ------- | ---- | -----------|----------- |
| 0x03   | 223    | 0x02 | 8 bit unsigned |8 bit unsigned |

**接收：**

Simple response

#### MSP_SET_BOUNDLESS 224

**发送：**

| device | command | size | fanThrd | fanUpThrd | hangCnt |
| ------ | ------- | ---- | -----------|-----------| -----------|
| 0x03   | 134    | 0x03 | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned |

**接收：**

Simple response

#### MSP_SET_SPRAY_VALUE 225

**发送：**

| device | command | size | waterPump | duration | startAngle | moveCnt |
| ------ | ------- | ---- | --------- | -------- | ---------- | ------- |
| 0x03   | 225    | 0x05 | 8 bit unsigned | 16 bit unsigned | 8 bit unsigned | 8 bit unsigned |

**接收：**

Simple response

#### MSP_SET_GYRO_THRESHOLD 226

**发送：**

| device | command | size | gyroDiffThreshold | gyroThreshold | gyroUpDiffThreshold | gyroUpThreshold |
| ------ | ------- | ---- | ----------- | ----------- | ----------- | ----------- |
| 0x03   | 136    | 0x04 | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned |

**接收：**

Simple response



