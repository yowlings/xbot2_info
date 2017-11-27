# Xbot ROS驱动包详细说明

## 源代码下载地址

Xbot一代机器人：[https://github.com/yowlings/xbot](https://github.com/yowlings/xbot)

Xbot2机器人：[https://github.com/yowlings/xbot2](https://github.com/yowlings/xbot2)

## 订阅的topics

| topic名称 | 消息格式 | 功能 |
| :--- | :--- | :--- |
| /mobile\_base/commands/velocity | geometry\_msgs/Twist | 机器人基础运动控制指令，线速度＋角速度 |
| /mobile\_base/commands/lift | xbot\_msgs/Lift | 机器人升降台高度控制，控制范围为0-100，按百分比计。上下极限位置各有限位开关，超过100按100计。 |
| /mobile\_base/commands/power | xbot\_msgs/Power | 机器人点击电源使能开关，布尔值，正常运动状态下默认为1，若要使其失效则发0 |
| /mobile\_base/commands/reset\_odometry | std\_msgs/Empty | 重置机器人odom数据，码盘计数为0，odom重置为原点坐标。 |
| /mobile\_base/commands/cloud\_camera | xbot\_msgs/CloudCamera | 控制机器人云台水平旋转角度以及摄像头俯仰角度，初始化为水平正视正前方，云台旋转角范围\(-90~90\)，摄像头俯仰角度范围\(-45~45\)，初始化或重置则都为\(0,0\)， |

## 发布的topics

| topic名称 | 消息格式 | 功能 |
| :--- | :--- | :--- |
| /mobile\_base/joint\_states | sensor\_msgs/JointState | 机器人活动关节的连接节点状态，包括轮子的旋转方位角状态等 |
| /mobile\_base/sensors/dock\_ir | xbot\_msgs/DockInfraRed | 机器人自主充电红外数据 |
| /mobile\_base/sensors/echo | xbot\_msgs/Echos | 六路超声数据 |
| /mobile\_base/sensors/imu | sensor\_msgs/Imu | 惯性单元数据，俯仰、滚转、偏航角度等，经过滤波后的数据 |
| /mobile\_base/sensors/imu\_data\_raw | xbot\_msgs/ImuNine | 惯性单元九路裸数据 |
| /mobile\_base/debug/sensors\_data | xbot\_msgs/DebugSensor | 用于调试输出制定传感器数据的topic |
| /mobile\_base/xbot/state | xbot\_msgs/XbotState | 监控机器人当前状态，包括电机电源状态，当前升降高度，当前云台角度，当前摄像头俯仰角度等。 |
| /odom | nav\_msgs/Odometry | 机器人odom |
| /tf | geometry\_msgs/Quaternoin | 机器人静态与odom动态tf消息 |





