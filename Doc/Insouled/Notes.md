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



<<<<<<< HEAD
#### pid
计算pid的基础过程
=======
### modeswitch_task
全局模式共有四种：
MANUAL_CTRL_MODE   =
SEMI_AUTO_MODE     =
AUTO_CTRL_MODE     =
SAFETY_MODE        = 安全模式

底盘模式共有七种：
CHASSIS_RELAX          =
CHASSIS_STOP           =
MANUAL_SEPARATE_GIMBAL = 手动模式底盘云台分离
MANUAL_FOLLOW_GIMBAL   = 手动模式底盘跟随云台
DODGE_MODE             =
AUTO_SEPARATE_GIMBAL   =
AUTO_FOLLOW_GIMBAL     =

sw1 左边手杆
sw2 右边手杆


mode_switch_task():[OS]
>>>>>>> 5003331853f9b98022a443ccf8bed00ea6983cd6

get_chassis_mode():
--如果底盘模式是躲避模式，则让twist_count(大概是用来计算已经转了多 少角度的计数器)清零。
--如果云台模式正在初始化，则让底盘模式变为停止模式
  否则，进入chassis_mode_handle()。

chassis_mode_handle():
--全局控制模式 = 手动模式：
    底盘模式变为手动模式底盘跟随云台
    如果km.twist_ctrl(大概与键盘输入有关)=1，则底盘模式变为躲避模式
  全局控制模式 = 半手动模式：
    左边手杆 上或中：接受来自电脑输入的底盘模式
    左边手杆 下   ：底盘模式=停止模式
  其他情况下，底盘模式=停止模式

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
