八座线控观光车主控程序
========================
内部控制协议
------------------------

|发送节点|接收节点|ID|数据长度|接收超时|byte[0]|byte[1]|byte[2]|byte[3]|byte[4]|byte[5]|byte[6]|byte[7]|
|----|------|-----|-----|-----|-----|-----|-----|----|-----|-----|-----|-----|
|主控|刹车模块|0x101|2||brake_speed(uint8_t)|brake_percent(uint8_t)||||||||
|主控|转向模块|0x103|7|target_position(int32_t)|target_position>>8(int32_t)|target_position>>16(int32_t)|target_position>>24(int32_t)|steering_zero_point(uint16_t)|steering_zero_point>>8(uint16_t)|max_steering_speed(uint8_t)||