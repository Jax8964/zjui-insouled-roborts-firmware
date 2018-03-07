### 2018.3.28


### info_get_task
 info_get_task()[OS]
#### info_interactive
  get_chassis_info()
    keyboard_chassis_hook()
##### remote_ctrl
    remote_ctrl_chassis_hook(): 初步处理遥控器数据，供chassis_task等使用.
  get_gimbal_info()
  get_shoot_info()
  get_global_last_info()
  ...


### chassis_task
负责根据模式计算底盘电机控制数据(?)
chassis_task()[OS]
| 底盘模式                     | 对应功能                 |
| ------------------------ | -------------------- |
| MANUAL_SEPARATE_GIMBAL   | 手动模式底盘云台分离           |
| MANUAL_FOLLOW_GIMBAL     | 手动模式底盘跟随云台           |

### modeswitch_task
负责模式切换
mode_switch_task(）[OS]



#### bsp_can
send_gimbal_cur(): 发送CAN1报文，云台电机电流。
send_chassis_cur(): 发送CAN1报文，底盘电机电流。

### comm_task
can_msg_send_task(): 发送控制信号[OS]
  send_gimbal_motor_ctrl_message()
  send_chassis_motor_ctrl_message():其中调用send_chassis_cur()
