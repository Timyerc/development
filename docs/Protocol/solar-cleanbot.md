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

* LEFT FRONT：左前超声波数据
* LEFT FRONT MID： 左前中超声波数据
* LEFT BACK： 左后超声波数据
* LEFT BACK MID： 左后中超声波数据
* RIGHT FRONT：右前超声波数据
* RIGHT FRONT MID： 右前中超声波数据
* RIGHT BACK： 右后超声波数据
* RIGHT BACK MID： 右后中超声波数据
* left  ratio: 左边超声波平均每秒更新数据次数
* right ratio: 右边超声波平均每秒更新数据次数

#### CMD_ATTITUDE 0x6C

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x6C    | 0x00 |

**接收：**

| command | size | yaw value | pitch value | roll value |
| ------- | ---- | ---------------|  ---------------|  ---------------|
| 0x6C    | 0x0A | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

* yaw value：解析后偏航角数据
* pitch value： 解析后俯仰角数据
* roll value： 解析后横滚角数据

#### CMD_GET_REVEIVER 0x6E

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x6E    | 0x00 |

**接收：**

| command | size | thr | yaw | mode | sucker | brush | NULL | fail |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------| 
| 0x6E    | 0x0E | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

* thr：前进遥杆数据
* yaw： 旋转遥杆数据
* mode： 解锁摇杆数据
* sucker： 模式选择摇杆数据
* NULL： 暂时发0
* fail： sbus数据出错标志

#### CMD_BATTERY_STATE 0x82

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x82    | 0x00 |

**接收：**

| command | size | battery value | battery state |
| ------- | ---- | ---------------| ---------------| 
| 0x82    | 0x08 | 16 bit unsigned  | 8 bit unsigned  |

* battery value： 电量
* battery state： 电池状态

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

* yaw gyro： 偏航角陀螺仪原始值
* pitch gyro：俯仰角陀螺仪原始值
* roll gyro：横滚角陀螺仪原始值
* yaw acc: 偏航角加速度原始值
* pitch acc: 俯仰角加速度原始值
* roll acc: 横滚角加速度原始值

#### CMD_GET_LINE_STATUS 0x67

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x67    | 0x00 |

**接收：**

| command | size | left edge dis | left edge Angle  | right edge dis | right edge Angle | front edge dis | front edge Angle | front dis | front angle | frame rate | state |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x67    | 0x13 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 8 bit unsigned  |

* left edge dis： 左边摄像机识别到的距离边沿的距离
* left edge Angle： 左边摄像机识别到的与边沿的角度
* right edge dis： 右边摄像机识别到的距离边沿的距离
* right edge Angle： 右边摄像机识别到的距离边沿的角度
* front edge dis： 前边摄像机识别前方距离垂直线的距离
* front edge Angle： 前边摄像机识别前方距离垂直线的角度
* front dis：前边摄像机识别前方边沿线的距离
* front angle： 前边摄像机识别前方边沿线的角度
* frame rate：帧数

|  state  |  value |
| ------- | ------ |
| 前边线有数据|  0x01 << 0 == 1 |
|   没有数据 |  0x01 << 1 == 1 |
| 右边线有数据 | 0x01 << 2 == 1 |
| 左边线有数据 | 0x01 << 3 == 1 |

#### CMD_MOTOR_SPEED 0x68

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x68    | 0x00 |

**接收：**

| command | size | left speed | right speed | front brush speed | break brush speed | average speed | move dis | odometer |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x68    | 0x18 | 32 bit unsigned  | 32 bit unsigned  | 32 bit unsigned  | 32 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 32 bit unsigned  |

* left speed: 左轮转速
* right speed： 右轮转速
* left brush speed：前滚刷转速
* break brush speed： 后滚刷转速
* average speed： 当前平均速度
* move dis：移动距离
* odometer： 自动模式总里程

#### CMD_CROSS_BATT 0x69

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x69    | 0x00 |

**接收：**

| command | size | null | move shift dis |
| ------- | ---- | ---------------|  ---------------|
| 0x69    | 0x0A | 16 bit unsigned  | 16 bit unsigned  |

* move shift dis：侧向偏移距离

#### CMD_SYSTEM 0x6D

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x6D    | 0x00 |

**接收：**

| command | size | Average System | task value | task value | task value |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|
| 0x6D    | 0x08 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

* AverageSystemPeriod ： 系统平均运行周期
* task period： 对应任务执行周期，这个任务是可以变的使用时需要先确定是哪个task

#### CMD_MOTOR_STATUS 0x70

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x70    | 0x00 |

**接收：**

| command | size | left motor faultType | right motor faultType | front brush faultType | break brush faultType |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|
| 0x70    | 0x04 | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  |

* left motor faultType： 左边驱动轮电机错误类型
* right motor faultType： 右边驱动轮电机错误类型
* front brush faultType： 前边滚刷电机错误类型
* break brush faultType： 后边混刷电机错误类型

#### CMD_DATA_POINT 0x83

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x83    | 0x00 |

**接收：**

| command | size | dis control out | angle ctrl out | merged yaw angle | along linear selected |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|
| 0x83    | 0x08 | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  | 16 bit unsigned  |

* 这个主要是用来调试用的，参数可根据需求更改这里列出一些例子
* dis control out：行驶距离
* angle ctrl out：行驶过程中角度换输出
* merged yaw angle: 偏航角
* along linear selected: 选中的视觉线

#### CMD_GET_MOVE_STATUS 0x85

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x03   | 0x85    | 0x00 |

**接收：**

| command | size | move mode | move state | move dir | is rotate flag | along linear selected | sbus channel value | sbus channel value |
| ------- | ---- | ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|  ---------------|
| 0x85    | 0x07 | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  | 8 bit unsigned  |

* 这些是一些调试信息
* move mode：运行模式
* move state：运行状态
* move dir：运行方向
* is rotate flag：是否旋转标志
* along linear selected： 选择的边沿线
* sbus channel value： sbus通道值