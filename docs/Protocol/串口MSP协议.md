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
| 0x03 | DCODE_SOLAR_CLEANBOT (VER_HW) |

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

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 97    | 0x00 |

**Response：**

| command | size | left front | right front | left back | right back | sample rate |
| ------- | ---- | -----------| ----------- | ----------| ---------- | ----------  |
| 97    | 0x0A | 16 bit unsigned  | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

#### CMD_SET_HEARTBEAT 98

**Require：**

| device | command | size | rotate value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 98    | 0x02 | 16 bit signed   |

**Response：**

Simple response

#### CMD_SET_ROTATE 99

**Require：**

| device | command | size | rotate value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 99    | 0x02 | 16 bit signed   |

**Response：**

Simple response

#### CMD_SET_SPEED 100

**Require：**

| device | command | size | speed value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 100    | 0x02 | 16 bit signed   |

**Response：**

Simple response

#### CMD_SET_LINE_STATUS 101

**Require：**

| device | command | size | busbar distance | busbar angle  | gap distance  | gap angle     | state          |
| ------ | ------- | ---- | ----------------| ------------- | ------------  | ------------- | -------------- |
| 0x03   | 101    | 0x08 | 16 bit signed   | 16 bit signed | 16 bit signed | 16 bit signed | 8 bit unsigned |

| state     | value |
| --------- | ----- |
| 正常       | 0x00  |
| 竖直有数据 | 0x01  |
| 水平有数据 | 0x02  |
| 异常       | 0x03  |

**Response：**

Simple response

#### CMD_MOTOR_SPEED 104

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 104    | 0x00 |

**Response：**

| command | size | left raw speed | right raw speed | average speed | move distance |
| ------- | ---- | ---------------| --------------- | --------------| ------------- |
| 104    | 0x0C | 32 bit signed  | 32 bit signed   | 16 bit signed | 16 bit signed |

#### CMD_ATTITUDE 108

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 108    | 0x00 |

**Response：**

| command | size | yaw | pitch | roll | merge yaw |
| ------- | ---- | ---------------| --------------- | --------------| ------------- |
| 108    | 0x08 | 16 bit signed  | 16 bit signed   | 16 bit signed | 16 bit signed |

#### CMD_GET_REVEIVER 110

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 110    | 0x00 |

**Response：**

| command | size | thr | yaw | arm | mode | sucker | brush | failsafe |
| ------- | ---- | --- | --- | ----| ---- | -------| ----- | -------- |
| 110    | 0x0E | 16 bit unsigned  | 16 bit unsigned   | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned | 16 bit unsigned |

#### CMD_FRAMERATE 111

**Require：**

| device | command | size | rotate value | 
| ------ | ------- | ---- | ----------------|
| 0x03   | 111    | 0x02 | 16 bit signed   |

**Response：**

Simple response

#### CMD_BATTERY_STATE 130

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 130    | 0x00 |

**Response：**

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

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 132    | 0x00 |

**Response：**

| command | size | temperature |
| ------- | ---- | ------------|
| 132    | 0x01 | 16 bit signed |

温度以10℃为一级

#### CMD_GET_MOVE_STATUS 133

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 133    | 0x00 |

**Response：**

| command | size | move mode | path state |
| ------- | ---- | ------------| -------- |
| 133    | 0x02 | 8 bit unsigned | 8 bit unsigned |

#### CMD_SET_CELL_INFO 134

**Require：**

| device | command | size | cell width | cell type | run dir  |
| ------ | ------- | ---- | -----------| --------- | -------- |
| 0x03   | 134    | 0x04 | 16 bit unsigned | 8 bit unsigned | 8 bit unsigned |

cell width: 电池片短边宽度

cell type: 0 半片；1 全片

run dir: 0 沿着长边运动；1 沿着短边运动

**Response：**

Simple response

## 擦窗机

### 擦窗机命令 Command

| 值 | 指令名称 | 描述 |
| ----- | ---- | ---- |
| 读取 |
| 102 | MSP_RAW_IMU | 陀螺仪加速度原始值 |
| 103 | MSP_DATA_POINT | 测试数据点 |
| 104 | MSP_GYRO_DETECT | 陀螺仪撞边判断 |
| 105 | MSP_EDGE_BOTTOM_DETECT | 底边检测 |
| 106 | MSP_MACHINE_STATE | 机器工作状态，水平or竖直 |
| 107 | MSP_THRESHOLD | 获取马达电流参数 |
| 108 | MSP_ATTITUDE | 机器姿态角 |
| 110 | MSP_ANALOG | AD数据 |
| 111 | MSP_ADAPTER | 适配器电压 |
| 114 | MSP_WATER_BOX | 水量检测 |
| 115 | MSP_WIFI_RSSI | WIFI产测结果 |
| 116 | MSP_WIFI_TEST | WIFI开始产测 |
| 117 | MSP_FAST_CURRENT | 直行电流判断 |
| 118 | MSP_Z_TURN_FAST_CURRENT | Z字水平换行电流 |
| 119 | MSP_BARO_DIFF | 无边气压检测 |
| 120 | MSP_SYSTICK | 系统周期 |
| 121 | MSP_OVO_WATER_DET | 水量检测 |
| 122 | MSP_LEAK_DET | 无边检测 |
| 130 | MSP_GET_PWMVALUE | 获取风机占比 |
| 131 | MSP_GET_USE_FAN_LEVEL_DYNAMIC_COMP | 获取风机最大最小占比 |
| 132 | MSP_GET_USE_FAN_OUTPUT_PID | 获取风机恒压占比 |
| 133 | MSP_GET_MOTOR_VALUE | 获取马达占比 |
| 134 | MSP_GET_BOUNDLESS | 获取无边电流 |
| 135 | MSP_GET_SPRAY_VALUE | 获取喷水功能 |
| 136 | MSP_GET_GYRO_THRESHOLD | 获取陀螺仪阈值 |
| 设置 |
| 205 | MSP_ACC_CALIBRATION | 加速度校准 |
| 208 | MSP_PLAY_VOICE | 测试语音 |
| 209 | MSP_SET_SPRAY | 控制喷水 |
| 211 | MSP_SET_FAN | 控制风机 |
| 214 | MSP_SET_MOTOR | 控制马达 |
| 215 | MSP_SET_TRIGGER | 发送触发信号 |
| 220 | MSP_SET_PWMVALUE | 设置风机占比 |
| 221 | MSP_SET_USE_FAN_LEVEL_DYNAMIC_COMP | 设置风机最大最小占比 |
| 222 | MSP_SET_USE_FAN_OUTPUT_PID | 设置风机恒压占比 |
| 223 | MSP_SET_MOTOR_VALUE | 设置马达占比 |
| 224 | MSP_SET_BOUNDLESS | 设置无边电流 |
| 225 | MSP_SET_SPRAY_VALUE | 设置喷水功能 |
| 226 | MSP_SET_GYRO_THRESHOLD | 设置陀螺仪阈值 |

### 擦窗机命令细节

#### MSP_GET_PWMVALUE 130

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 130    | 0x00 |

**Response：**

| device | command | size | pwmValue |
| ------ | ------- | ---- | -----------|
| 0x03   | 130    | 0x01 | 8 bit unsigned |

#### MSP_GET_USE_FAN_LEVEL_DYNAMIC_COMP 131

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 131    | 0x00 |

**Response：**

| device | command | size | pwmvalueMax | pwmvalueMin|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 131    | 0x02 | 8 bit unsigned |8 bit unsigned |

#### MSP_GET_USE_FAN_OUTPUT_PID 132

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 132    | 0x00 |

**Response：**

| device | command | size | fanPwmvalueAtIdle | fanPwmvalueMin|fanPwmvalueMax|defaultTargetFanPwmvalue|maxTargetFanPwmvalue|minTargetFanPwmvalue|
| ------ | ------- | ---- | -----------|-----------|------- | ---- | -----------|-----------|
| 0x03   | 132    | 0x09 | 8 bit unsigned |8 bit unsigned |8 bit unsigned |16 bit unsigned |16 bit unsigned |16 bit unsigned |

#### MSP_GET_MOTOR_VALUE 133

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 133    | 0x00 |

**Response：**

| device | command | size | minPwmValue | upMinPwmValue|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 133    | 0x02 | 8 bit unsigned |8 bit unsigned |

#### MSP_GET_BOUNDLESS 134

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 134    | 0x00 |

**Response：**

| device | command | size | fanThrdAddE | fanUpThrdAdd|hangCnt|
| ------ | ------- | ---- | -----------|-----------| -----------|
| 0x03   | 134    | 0x03 | 8 bit unsigned |8 bit unsigned |8 bit unsigned |

#### MSP_GET_SPRAY_VALUE 135

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 135    | 0x00 |

**Response：**

| device | command | size | waterPump | waterPumpDuration|waterPumpStartAngle|waterPumpMoveCnt|
| ------ | ------- | ---- | -----------|-----------| -----------|-----------|
| 0x03   | 135    | 0x05 | 8 bit unsigned |16 bit unsigned |8 bit unsigned |8 bit unsigned |

#### MSP_GET_GYRO_THRESHOLD 136

**Require：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 136    | 0x00 |

**Response：**

| device | command | size | gyroDiffThreshold | gyroThreshold|gyroUpDiffThreshold|gyroUpThreshold|
| ------ | ------- | ---- | -----------|-----------| -----------|-----------|
| 0x03   | 136    | 0x04 | 8 bit unsigned |8 bit unsigned |8 bit unsigned |8 bit unsigned |

#### MSP_SET_PWMVALUE 220

**Require：**

| device | command | size | pwmValue |
| ------ | ------- | ---- | -----------|
| 0x03   | 220    | 0x01 | 8 bit unsigned |

**Response：**

Simple response

#### MSP_SET_USE_FAN_LEVEL_DYNAMIC_COMP 221

**Require：**

| device | command | size | pwmvalueMax | pwmvalueMin|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 221    | 0x02 | 8 bit unsigned |8 bit unsigned |

**Response：**

Simple response

#### MSP_SET_USE_FAN_OUTPUT_PID 222

**Require：**

| device | command | size | fanPwmvalueAtIdle | fanPwmvalueMin|fanPwmvalueMax|defaultTargetFanPwmvalue|maxTargetFanPwmvalue|minTargetFanPwmvalue|
| ------ | ------- | ---- | -----------|-----------|------- | ---- | -----------|-----------|
| 0x03   | 222    | 0x09 | 8 bit unsigned |8 bit unsigned |8 bit unsigned |16 bit unsigned |16 bit unsigned |16 bit unsigned |

**Response：**

Simple response

#### MSP_SET_MOTOR_VALUE 223

**Require：**

| device | command | size | minPwmValue | upMinPwmValue|
| ------ | ------- | ---- | -----------|-----------|
| 0x03   | 223    | 0x02 | 8 bit unsigned |8 bit unsigned |

**Response：**

Simple response

#### MSP_SET_BOUNDLESS 224

**Require：**

| device | command | size | fanThrdAddE | fanUpThrdAdd|hangCnt|
| ------ | ------- | ---- | -----------|-----------| -----------|
| 0x03   | 134    | 0x03 | 8 bit unsigned |8 bit unsigned |8 bit unsigned |

**Response：**

Simple response

#### MSP_SET_SPRAY_VALUE 225

**Require：**

| device | command | size | waterPump | waterPumpDuration|waterPumpStartAngle|waterPumpMoveCnt|
| ------ | ------- | ---- | -----------|-----------| -----------|-----------|
| 0x03   | 225    | 0x05 | 8 bit unsigned |16 bit unsigned |8 bit unsigned |8 bit unsigned |

**Response：**

Simple response

#### MSP_SET_GYRO_THRESHOLD 226

**Require：**

| device | command | size | gyroDiffThreshold | gyroThreshold|gyroUpDiffThreshold|gyroUpThreshold|
| ------ | ------- | ---- | -----------|-----------| -----------|-----------|
| 0x03   | 136    | 0x04 | 8 bit unsigned |8 bit unsigned |8 bit unsigned |8 bit unsigned |

**Response：**

Simple response



