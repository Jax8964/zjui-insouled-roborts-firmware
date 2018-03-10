### 2018.3.28


### info_get_task
 info_get_task()[OS]
#### info_interactive
  get_chassis_info()
##### remote_ctrl
    remote_ctrl_chassis_hook(): 初步处理遥控器数据，供chassis_task等使用.
  get_gimbal_info()
  get_shoot_info()
  get_global_last_info()
  ...

### modeswitch_task
  负责模式切换
  mode_switch_task(）[OS]

glb_ctrl_mode: 主模式，sw2

### chassis_task
负责根据模式计算底盘电机控制数据(?)
chassis_task()[OS]
| 底盘模式                     | 对应功能                 |
| ------------------------ | -------------------- |
| MANUAL_SEPARATE_GIMBAL   | 手动模式底盘云台分离           |
| MANUAL_FOLLOW_GIMBAL     | 手动模式底盘跟随云台           |

#### pid
计算pid的基础过程



#### bsp_can
send_gimbal_cur(): 发送CAN1报文，云台电机电流。
send_chassis_cur(): 发送CAN1报文，底盘电机电流。

### comm_task
can_msg_send_task(): 发送控制信号[OS]
  send_gimbal_motor_ctrl_message()
  send_chassis_motor_ctrl_message():其中调用send_chassis_cur()


err_detector_hook: 记录当前时间，可能是用于debug

rc.ch2/ch1/ch3--(chassis_operation_func)--rm.vx/vy/vw-->chassis.vx/vy/vw--(mecanum_calc)-->chassis.wheel_speed_ref

moto_chassis[i].speed_rpm--(get_chassis_info)-->chassis.wheel_speed_fdb
