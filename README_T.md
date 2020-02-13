八座线控观光车主控程序
========================
内部控制协议
------------------------

|发送节点|接收节点|ID|数据长度|接收超时|byte[0]|byte[1]|byte[2]|byte[3]|byte[4]|byte[5]|byte[6]|byte[7]|
|----|------|-----|-----|-----|-----|-----|-----|----|-----|-----|-----|-----|
|主控|刹车模块|0x101|2||brake_speed(uint8_t)|brake_percent(uint8_t)||||||||
|主控|转向模块|0x103|7||target_position(int32_t)|target_position>>8(int32_t)|target_position>>16(int32_t)|target_position>>24(int32_t)|steering_zero_point(uint16_t)|steering_zero_point>>8(uint16_t)|max_steering_speed(uint8_t)||
|主控|驻车模块|0x104|2||parking_request(int8_t)|max_parking_current(int8_t)||||||||
|主控|英博尔驱动模块|0x305|8||dirving_mode and shift|motor_speed>>8(uint16_t)|motor_speed(uint16_t)|driving_acc(uint8_t)|dirving_dec(uint8_t)|0|0|data_checksums|