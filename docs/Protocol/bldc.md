# 无刷电机通信协议

## 核心命令 Core Command

| 值 | 名称 |
| ----- | ---- |
| 0x01 | OSP_ver |
| 0x66 | OSP_HALL |
| 0x6D | OSP_SYSTEM |
| 0x6F | OSP_ANALOG |
| 0x70 | OSP_ADC |
| 0x71 | OSP_FAULT |
| 0x8D | OSP_SET_PID |
| 0x8F | OSP_PID_DATA |

### 无刷电机命令细节 ###

#### OSP_ver 0x01

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x01    | 0x00 |

**接收：**

| command | size | VER_HW | VER_MOA_MAJOR | VER_MOA_MINOR | VER_MOA_PATCH | VER_API_MAJOR |VER_API_MINOR |VER_BUILD_CNT |desktopFun FLAG |
| ------- | ---- | -----------| ----------- | ----------| ---------- | ----------  | ----------  | ----------  | ----------  |
| 0x01    | 0x08 | 8 bit unsigned  | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned | 8 bit unsigned |

#### OSP_HALL 0x66

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x66    | 0x00 |

**接收：**

| command | size | hall state |
| ------- | ---- | ---------------| 
| 0x66    | 0x01 | 8 bit unsigned  |

#### OSP_SYSTEM 0x6D

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x6D    | 0x00 |

**接收：** 
*** task  的类型是可变的不统一 ***

| command | size | AverageSystemPeriod | task period | task period |
| ------- | ---- | ---------------| --------------- | --------------|
| 0x6D    | 0x0A | 16 bit unsigned  | 32 bit unsigned   | 32 bit unsigned |

#### OSP_ANALOG 0x6F

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x6F    | 0x00 |

**接收：**

NO response

#### OSP_ADC 0x70

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x70    | 0x00 |

**接收：**

| command | size | adc Sumavg | adc Vdc |
| ------- | ---- | ---------------| --------------- |
| 0x70    | 0x04 | 16 bit unsigned  | 16 bit unsigned   |

#### OSP_FAULT 0x71

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x71    | 0x00 |

**接收：**

| command | size | fault |
| ------- | ---- | ---------------|
| 0x71    | 0x01 | 8 bit unsigned  |

#### OSP_SET_PID 0x8D

**发送：**

| device | command | size |        kp        |        ki        |        kd        |
| ------ | ------- | ---- | ---------------- | ---------------- | ---------------- |
| 0x05   | 0x8D    | 0x0C | 32 bit unsigned  | 32 bit unsigned  | 32 bit unsigned  |

**接收：**

Simple response

#### OSP_PID_DATA 0x8F

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x8F    | 0x00 |

**接收：**

| command | size | target speed value | actual speed value |
| ------- | ---- | ---------------| --------------- |
| 0x8F    | 0x08 | 32 bit unsigned  | 32 bit unsigned   |
