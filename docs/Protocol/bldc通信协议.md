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

desktopFun FLAG： 上位机识别无刷电机的标志

#### OSP_HALL 0x66

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x66    | 0x00 |

**接收：**

| command | size | hall state |
| ------- | ---- | ---------------| 
| 0x66    | 0x01 | 8 bit unsigned  |

* hall state: hall当前值

#### OSP_SYSTEM 0x6D

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x6D    | 0x00 |

**接收：** 

| command | size | AverageSystemPeriod | task period | task period |
| ------- | ---- | ---------------| --------------- | --------------|
| 0x6D    | 0x0A | 16 bit unsigned  | 32 bit unsigned   | 32 bit unsigned |

* AverageSystemPeriod ： 系统平均运行周期
* task period： 对应任务执行周期，这个任务是可以变的使用时需要先确定是哪个task

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

| command | size |  sumavg value | vdc value |
| ------- | ---- | ---------------| --------------- |
| 0x70    | 0x04 | 16 bit unsigned  | 16 bit unsigned   |

* sumavg value：回路电流，基本等于电机总耗电
* vdc value ： 电机测量到的母线电压

#### OSP_FAULT 0x71

**发送：**

| device | command | size |
| ------ | ------- | ---- |
| 0x05   | 0x71    | 0x00 |

**接收：**

| command | size | fault |
| ------- | ---- | ---------------|
| 0x71    | 0x01 | 8 bit unsigned  |

| fault | value |
| ------ | ---- |
| 捕获外部pwm失败 |  0  |
| 电机电流异常    |  1  |
| 母线电压异常    |  2  |
| hall状态异常    |  3  |

#### OSP_SET_PID 0x8D

**发送：**

| device | command | size |        kp        |        ki        |        kd        |
| ------ | ------- | ---- | ---------------- | ---------------- | ---------------- |
| 0x05   | 0x8D    | 0x0C | 32 bit unsigned  | 32 bit unsigned  | 32 bit unsigned  |

* kp: pid比例项
* ki：pid积分项
* kd: pid微分项

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

* target speed value：捕获到的速度
* actual speed value：电机实时采集到的速度
