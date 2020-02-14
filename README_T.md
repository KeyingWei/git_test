八座线控观光车主控程序
========================
内部控制协议
------------------------
* 主控下发控制帧

|发送节点|接收节点|ID|数据长度|接收超时|byte[0]|byte[1]|byte[2]|byte[3]|byte[4]|byte[5]|byte[6]|byte[7]|备注|
|----|------|-----|-----|-----|-----|-----|-----|----|-----|-----|-----|-----|-----|
|主控|刹车模块|0x101|2||brake_speed(uint8_t)|brake_percent(uint8_t)|||||||||
|主控|转向模块|0x103|7||target_position(int32_t)|target_position>>8(int32_t)|target_position>>16(int32_t)|target_position>>24(int32_t)|steering_zero_point(uint16_t)|steering_zero_point>>8(uint16_t)|max_steering_speed(uint8_t)|||
|主控|驻车模块|0x104|2||parking_request(int8_t)|max_parking_current(int8_t)|||||||||
|主控|英博尔驱动模块|0x305|8||dirving mode and gear|motor_speed>>8(uint16_t)|motor_speed(uint16_t)|driving_acc(uint8_t)|dirving_dec(uint8_t)|0|0|data_checksums|byte0[0:2]mode:<br>0-manu_mode,<br>1-auto_mode<br>byte0[3:5]gear:<br>0-P_Gear,<br>1-N_Gear,<br>2-D_gear,<br>3-R_Gear|
|主控|所有模块|0x401|1||module_auto_status(uint8_t)||||||||bit[0]:braking bit[1]:parking bit[2]:steering bit[3]:driving bit[4]:gear bit[5]:light |
|主控|所有模块|0x2AA|1||0XAA||||||||该帧用于当转向发送人工干预信息后，主控发送的应答信号|

* 模块心跳帧

|发送节点|接收节点|ID|数据长度|接收超时|byte[0]|byte[1]|byte[2]|byte[3]|byte[4]|byte[5]|byte[6]|byte[7]|备注|
|----|------|-----|-----|-----|-----|-----|-----|----|-----|-----|-----|-----|-----|
|刹车模块|主控|0x201|4|200ms|brake_percent(uint8_t)|real_percent(uint8_t)|auto_driver_flag(uint8_t)|error_flag(uint8_t)|||||error_flag[0:3]:<br>bit[0]-电机过流错误<br>bit[1]-光电传感器错误<br>bit[2]-电机编码器错误<br>bit[3]-驱动错误|
|转向模块|主控|0x203|8|500ms|encoder_value(uint32_t)|encoder_value>>8(uint32_t)|encoder_value>>16(uint32_t)|encoder_value>>24(uint32_t)|steering_speed(int16_t)|steering_speed>>8(int16_t)|error_flag|auto_driver_flag|error_flag[0:2]:<br>bit[0]-扭矩传感器自检错误<br>bit[1]-编码器错误<br>bit[2]-电机过流错误|
|驻车模块|主控|0x204|4|200ms|parking_status(uint8_t)|parking_request(uint8_t)|real_current(uint8_t)|error_flag(uint8_t)|||||error_flag[0:3]:<br>bit[0]-电机过流错误<br>|
|英博尔驱动模块|主控|0x306|8||dirving mode and gear|motor_speed>>8(uint16_t)|motor_speed(uint16_t)|driving_acc(uint8_t)|dirving_dec(uint8_t)|motor_temp|error_flag|data_checksums|error_flag:<br>0x2-预充电故障(上电预充不正常),<br>0x3-过流故障<br>0x4-MCU过温故障,<br>0x5-主接触器丢失,<br>0x6-电流传感器故障,<br>0x7-编码器器故障<br>0x8-CAN通讯故障,<br>0x9-母线欠压故障,<br>0xa-母线过压故障,<br>0xb-电机过温故障,<br>0xc-EEPROM读写故障,<br>0xd加速器故障,<br>0xf-电机堵转故障|
|测电压单元|主控|0x107|6|200ms|0|0|real_speed>>8(int16_t)|real_speed(int16_t)|||||单位：cm/s|
|测速单元|主控|0x108|6|200ms|real_vlot>>8(uint16_t)|real_vlot(uint16_t)|0|0|||||单位：V|

*异常反馈帧
|发送节点|接收节点|ID|数据长度|接收超时|byte[0]|byte[1]|byte[2]|byte[3]|byte[4]|byte[5]|byte[6]|byte[7]|备注|
|----|------|-----|-----|-----|-----|-----|-----|----|-----|-----|-----|-----|-----|
|转向模块|主控|0x00A|1||0|||||||||
