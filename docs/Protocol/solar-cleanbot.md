# 光伏清洁机器人通信协议

#### 版本号按照规定应该改为0x04但是现在主板的程序还没更改先用0x03后续做更新

## 核心命令 Core Command

#### 测试主板用到的
| 值 | 名称 |
| ----- | ---- |
| 0x61 | CMD_RANGEFINDER |
| 0x6C | CMD_ATTITUDE |
| 0x6E | CMD_GET_REVEIVER |
| 0x82 | CMD_BATTERY_STATE |

#### 其余指令

| 值 | 名称 |
| ----- | ---- |
| 0x66 | CMD_RAW_IMU |
| 0x67 | CMD_GET_LINE_STATUS |
| 0x68 | CMD_MOTOR_SPEED |
| 0x69 | CMD_CROSS_BATT |
| 0x6D | CMD_SYSTEM |
| 0x70 | CMD_MOTOR_STATUS |
| 0x83 | CMD_DATA_POINT |
| 0x85 | CMD_GET_MOVE_STATUS |

### 测试主板需要用到的命令与回应

#### CMD_RANGEFINDER 0x61

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x61    | 0x00 |

**接收：**

| command | size | LEFT FRONT | LEFT FRONT MID | LEFT BACK | LEFT BACK MID | RIGHT FRONT | RIGHT FRONT MID | RIGHT BACK | RIGHT BACK MID | left ratio | right ratio |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------| 
| 0x66    | 0x14 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_ATTITUDE 0x6C

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x6C    | 0x00 |

**接收：**

| command | size | yaw value | pitch value | roll value |
| ------- | ---- | ---------------|  ---------------|  ---------------|
| 0x6C    | 0x0A | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_GET_REVEIVER 0x6E

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x6E    | 0x00 |

**接收：**

| command | size | thr | yaw | mode | sucker | brush | NULL | fail |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------| 
| 0x6E    | 0x0E | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_BATTERY_STATE 0x82

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x82    | 0x00 |

**接收：**

| command | size | battery value | battery state |
| ------- | ---- | ---------------| ---------------| 
| 0x82    | 0x08 | 16 bit unsigned  | 8 bit unsigned  |


### 其余命令与回应

#### CMD_RAW_IMU 0x66

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x66    | 0x00 |

**接收：**

| command | size | yaw gyro | pitch gyro | roll gyro | yaw acc | pitch acc | roll acc |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x66    | 0x0C | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_GET_LINE_STATUS 0x67

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x67    | 0x00 |

**接收：**

| command | size | left edge dis | left edge Angle  | right edge dis | right edge Angle | front edge dis | front edge Angle | front dis | front angle | frame rate | state |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x67    | 0x13 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 8 bit unsigned  |

#### CMD_MOTOR_SPEED 0x68

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x68    | 0x00 |

**接收：**

| command | size | left speed | right speed | left brush speed | right brush speed | average speed | move dis | odometer |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x68    | 0x18 | 32 bit unsigned  | 32 bit unsigned  | 32 bit unsigned  | 32 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 32 bit unsigned  |

#### CMD_CROSS_BATT 0x69

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x69    | 0x00 |

**接收：**

| command | size | null | move shift dis |
| ------- | ---- | ---------------|  ---------------|
| 0x69    | 0x0A | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_SYSTEM 0x6D

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x6D    | 0x00 |

**接收：**

| command | size | Average System | task value | task value | task value |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|
| 0x6D    | 0x08 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_MOTOR_STATUS 0x70

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x70    | 0x00 |

**接收：**

| command | size | left motor faultType | right motor faultType | left brush faultType | right brush faultType |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|
| 0x70    | 0x04 | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  |

#### CMD_DATA_POINT 0x83

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x83    | 0x00 |

**接收：**

| command | size | dis control out | angle ctrl out | merged yaw angle | along linear selected |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|
| 0x83    | 0x08 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

#### CMD_GET_MOVE_STATUS 0x85

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x85    | 0x00 |

**接收：**

| command | size | move mode | move state | move dir | is rotate flag | along linear selected | sbus channel value | sbus channel value |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x85    | 0x07 | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  |