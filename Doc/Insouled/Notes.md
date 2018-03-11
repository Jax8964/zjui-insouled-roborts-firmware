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

### gimbal_task
no_action_handle():
--gim.input.no_action_flag = 1:

### imu_task
invSqrt(x):
  较快地求出 1/sqrt(x) 可以用来求法线
init_quaternion():
  对四元数初始化
imu_AHRS_update():
  当前航姿更新
imu_attitude_update():
  当前姿势更新
imu_param_init():
  imu参数初始化
imu_temp_ctrl_init()
imu_temp_keep()


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

get_main_ctrl_mode():
--如果电脑连接：
  右拨杆 上 MANUAL_CTRL_MODE 手动
        ## SAFETY_MODE      安全
        中 SEMI_AUTO_MODE   半手动
        下 AUTO_CTRL_MODE   自动
--如果电脑没有连接：
  右拨杆 上:     手动
        中下:   安全
--如果左拨杆 下 并且 右拨杆 下：
    安全模式
--kb_enable_hook();
  左中 并且 右上 则 km.kb_enable = 1

get_chassis_mode():
--如果底盘模式是躲避模式，则   让twist_count(大概是用来计算已经转了多少角度的计数器)清零。
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

云台模式：
GIMBAL_RELAX         = 0, 放松模式
GIMBAL_INIT          = 1, 初始化模式
GIMBAL_NO_ARTI_INPUT = 2, 没有输入模式
GIMBAL_FOLLOW_ZGYRO  = 3, 跟随陀螺仪
GIMBAL_TRACK_ARMOR   = 4,
GIMBAL_PATROL_MODE   = 5, 巡逻模式
GIMBAL_SHOOT_BUFF    = 6,
GIMBAL_POSITION_MODE = 7,

gim.input.action_mode = 1, 云台输入有效
gim.input.action_mode = 0, 云台输入无效

get_gimbal_mode():
--判断云台有没有输入（在gimbal_task中有用到）gim.input.action_mode = 0 or 1
--如果gim.ctrl_mode不是初始化模式，进入gimbal_mode_handle();
--如果gim.ctrl_mode不是巡逻模式，patrol_count(在gimbal_patrol_handle()中用于计算) = 0;
--如果gim.last_ctrl_mode是放松模式，但是现在的模式不是放松模式，那么：
      gim.ctrl_mode = GIMBAL_INIT
      初始化yaw的角度：gim.ecd_offset_angle = gim.sensor.yaw_relative_angle(大概是目前云台yaw角度，似乎是可以直接获得的？)
      gimbal_back_param():(我觉得是将gimbal的一系列参数初始化)


gimbal_mode_handle():
--如果glb_ctrl_mode 是手动模式：
    若last_glb_ctrl_mode = 半手动， 底盘模式变为 GIMBAL_FOLLOW_ZGYRO
    若云台没有输入(gim.input.action_mode = 0) 且,则：



#### bsp_can
send_gimbal_cur(): 发送CAN1报文，云台电机电流。
send_chassis_cur(): 发送CAN1报文，底盘电机电流。

### comm_task
can_msg_send_task(): 发送控制信号[OS]
  send_gimbal_motor_ctrl_message()
  send_chassis_motor_ctrl_message():其中调用send_chassis_cur()

###keyboard


err_detector_hook: 记录当前时间，可能是用于debug

rc.ch2/ch1/ch3--(chassis_operation_func)--rm.vx/vy/vw-->chassis.vx/vy/vw--(mecanum_calc)-->chassis.wheel_speed_ref

moto_chassis[i].speed_rpm--(get_chassis_info)-->chassis.wheel_speed_fdb
