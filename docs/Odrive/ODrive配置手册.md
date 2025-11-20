# ODrve调试手册

## 1、连接好电机线、制动电阻、电源线、USB、CAN调试工具

![image](image\OdriveTest0.jpg)

## 2、下载固件

![](image\OdriveTest2.png)

## 3、进入终端，打开Odrivetool工具，进行电机参数校准

![image](image\OdriveTest1.png)

## 4、电机参数校准，通过Odrvtool工具进行

**1、清除所有参数，恢复默认设置**

```
odrv0.erase_configuration()   #擦除数据
```

**2、参数设置并保存**

```
#驱动板基础配置：
odrv0.config.brake_resistance = 2
odrv0.config.dc_bus_undervoltage_trip_level = 8
odrv0.config.dc_bus_overvoltage_trip_level = 56
odrv0.config.dc_max_positive_current = 15
odrv0.config.dc_max_negative_current = -15
odrv0.config.max_regen_current = 0

#M0电机参数设置
odrv0.axis0.motor.config.pole_pairs = 2
odrv0.axis0.motor.config.calibration_current = 5
odrv0.axis0.motor.config.resistance_calib_max_voltage = 10
odrv0.axis0.motor.config.motor_type =0                      # MOTOR_TYPE_HIGH_CURRENT
odrv0.axis0.motor.config.current_control_bandwidth = 10
odrv0.axis0.motor.config.current_lim = 15
odrv0.axis0.motor.config.requested_current_range = 90
#M1电机参数
odrv0.axis1.motor.config.pole_pairs = 2
odrv0.axis1.motor.config.calibration_current = 5
odrv0.axis1.motor.config.resistance_calib_max_voltage = 10
odrv0.axis1.motor.config.motor_type =0# MOTOR_TYPE_HIGH_CURRENT
odrv0.axis1.motor.config.current_control_bandwidth = 10
odrv0.axis1.motor.config.current_lim = 15
odrv0.axis1.motor.config.requested_current_range = 90

#M0编码器参数
odrv0.axis0.encoder.config.mode = ENCODER_MODE_HALL
odrv0.axis0.encoder.config.cpr = 12
odrv0.axis0.encoder.config.bandwidth = 100
#M1编码器参数
odrv0.axis1.encoder.config.mode = ENCODER_MODE_HALL
odrv0.axis1.encoder.config.cpr = 12
odrv0.axis1.encoder.config.bandwidth = 100

#M0控制器配置，速度闭环模式
odrv0.axis0.controller.config.control_mode = CONTROL_MODE_VELOCITY_CONTROL
odrv0.axis0.controller.config.vel_limit = 57
odrv0.axis0.controller.config.pos_gain = 0
odrv0.axis0.controller.config.vel_gain = 0.06
odrv0.axis0.controller.config.vel_integrator_gain = 0.2
odrv0.axis0.controller.config.input_mode = INPUT_MODE_PASSTHROUGH
odrv0.axis0.controller.config.vel_ramp_rate = 100
#M1控制器配置，速度闭环模式
odrv0.axis1.controller.config.control_mode = CONTROL_MODE_VELOCITY_CONTROL
odrv0.axis1.controller.config.vel_limit = 57
odrv0.axis1.controller.config.pos_gain = 0
odrv0.axis1.controller.config.vel_gain = 0.06
odrv0.axis1.controller.config.vel_integrator_gain = 0.2
odrv0.axis1.controller.config.input_mode = INPUT_MODE_PASSTHROUGH
odrv0.axis1.controller.config.vel_ramp_rate = 100
odrv0.save_configuration()
odrv0.reboot()
```

**3、M0电机校准**

```
#M0电机和编码器校准：
odrv0.axis0.requested_state=AXIS_STATE_MOTOR_CALIBRATION                #电机自检

odrv0.axis0.requested_state = AXIS_STATE_ENCODER_OFFSET_CALIBRATION     #编码器自检

odrv0.axis0.requested_state=AXIS_STATE_CLOSED_LOOP_CONTROL              #进入闭环模式

odrv0.axis0.motor.config.pre_calibrated=True                            #保存电机自检
odrv0.axis0.encoder.config.pre_calibrated=True                          #保存编码器自检
odrv0.axis0.config.startup_closed_loop_control=True                     #开机进入闭环
odrv0.axis0.requested_state=AXIS_STATE_IDLE                             #进入空闲模式
#odrv0.axis0.config.startup_encoder_offset_calibration =True            #开机自检编码器，开机后自转
odrv0.save_configuration()
odrv0.reboot()
```

**4、M0电机校准**

```
#M1电机和编码器校准：
odrv0.axis1.requested_state=AXIS_STATE_MOTOR_CALIBRATION                #电机自检

odrv0.axis1.requested_state = AXIS_STATE_ENCODER_OFFSET_CALIBRATION     #编码器自检

odrv0.axis1.requested_state=AXIS_STATE_CLOSED_LOOP_CONTROL              #进入闭环模式

odrv0.axis1.motor.config.pre_calibrated=True                            #保存电机自检
odrv0.axis1.encoder.config.pre_calibrated=True                          #保存编码器自检
odrv0.axis1.config.startup_closed_loop_control=True                     #开机进入闭环
odrv0.axis1.requested_state=AXIS_STATE_IDLE                             #进入空闲模式
#odrv0.axis1.config.startup_encoder_offset_calibration =True            #开机自检编码器，开机后自转
odrv0.save_configuration()
odrv0.reboot()
```

**5、CAN设置**

```
#使用ODrivetool配置Can
odrv0.axis0.config.can_node_id = 0x001
odrv0.axis1.config.can_node_id = 0x001
odrv0.can.set_baud_rate(100000)
odrv0.save_configuration()
odrv0.reboot()
```

**6、转速调试**

```
#M0给定目标速度：
odrv0.axis0.controller.input_vel = -50
odrv0.axis1.controller.input_vel = -50

odrv0.axis0.controller.input_vel = 0
odrv0.axis1.controller.input_vel = 0

odrv0.axis0.controller.input_vel = 10
odrv0.axis1.controller.input_vel = 10
```

**7、异常处理**

```
dump_errors(odrv0)                  #查看错误

odrv0.axis0.error=0                 #清除轴错误
odrv0.axis0.motor.error=0           #清除电机错误
odrv0.axis0.encoder.error = 0       #清除编码器错误

odrv0.axis1.error=0                 #清除轴错误
odrv0.axis1.motor.error=0           #清除电机错误
odrv0.axis1.encoder.error = 0       #清除编码器错误

odrv0.axis0.requested_state=AXIS_STATE_IDLE #空闲模式
odrv0.axis1.requested_state=AXIS_STATE_IDLE #空闲模式

odrv0.axis0.requested_state = AXIS_STATE_CLOSED_LOOP_CONTROL #闭环模式
odrv0.axis1.requested_state = AXIS_STATE_CLOSED_LOOP_CONTROL #闭环模式
```

**8、ODrive参数查询**
通常情况下，设置类命令不带参数，即可查看设置值

```
odrv0.axis0.encoder.config.hall_polarity_calibrated     #读取极性值

odrv0.axis0.encoder.config.hall_phase_shift_calibrated  #读取相位值

odrv0.axis0.motor.config.phase_inductance               #查看电感

odrv0.axis0.motor.config.phase_resistance               #查看电阻

odrv0.axis1.motor.config.phase_inductance               #查看电感

odrv0.axis1.motor.config.phase_resistance               #查看电阻

odrv0.axis0.controller.input_vel                        #查看设置转速

odrv0.axis0.encoder.hall_state                          #查看霍尔状态

odrv0.axis1.encoder.hall_state                          #查看霍尔状态

odrv0.axis0.motor.current_control.Iq_measured           #查看M0电流

odrv0.axis1.motor.current_control.Iq_measured           #查看M1电流
```

## 5、补充

ODrive手册：[Getting Started — ODrive Documentation 0.6.11 documentation](https://docs.odriverobotics.com/v/latest/guides/getting-started.html)