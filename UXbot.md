# UXbot机器人

## 一、机器人简介

xbot系列机器人是重德智能科技出品的一款包含机器人硬件平台、软件平台和操作系统支持三方面在内的机器人平台解决方案。该机器人经历了xbot1与xbot2两代机器人的开发与改进，目前已完全支持室内环境中的所有的机器人相关传感器接入和机器人应用。该平台自带运动控制系统、二维激光雷达点云测距、超声测距、红外测距、升降平台控制器、高清摄像头（RGB、深度可选）以及机器人视觉伺服云台。另外，由于机器人搭载了高性能的处理器和N系列高性能显卡，支持cuda加速以及运行绝大部分运算需求较高的应用，并且对于ROS的完全支持使得机器人可以接入任何支持ROS框架的硬件传感器或者其他开源软件。xbot2机器人平台将是您室移动内机器人应用的不二选择。

UXbot机器人是重德智能推出的专门面向高校的科研教学机器人平台University Xbot。作为Xbot2机器人的一款基础版产品，UXbot拥有几乎所有的Xbot2的功能，但是价格却十分亲民，重德智能力求将最具有性价比的科研教学机器人平台带给更多的高校师生，为中国的机器人教育事业贡献自己的力量。

## 二、产品特点

- ### 稳定、可靠的运动控制

  Xbot机器人运动电机采用先进的PID鲁棒性控制算法，提供十分稳定、可靠的机器人运动控制，配以高减速比的高精度电机，机器人运动速度可控制到0.01m/s的精度，最小速度达0.01m/s，最大速度超2m/s。具有加速时间短，制动效果明显等的多方面优秀特性。

- ### 完备的驱动软件支持

  我们为UXbot机器人提供完备的驱动软件，采用国际通用的驱动软件框架和通信协议，能够提供50Hz频率以上的数据心跳包传输和快速精准地数据编码解码功能，使机器人的运动状态控制精度到达20ms以上，从而机器人能够更加迅速地相应用户算法的控制。

- ### 超长的续航能力

  UXbot机器人配备高达60Ah的超大容量电池，续航时间最高可达15小时。同时，在软硬件上都配备了机器人电池使用情况监控功能。

  在硬件上，机器人本体面板上安装了实时显示电池放电功率、电压、电流等信息的液晶面板，非常直观方便地显示机器人的用电情况。

  在软件方面，机器人拥有电压电流监控消息节点，实时推送机器人当前的电量百分比情况。另外，在机器人配备的机器人交互显示平板上也同步显示机器人当前的电量情况。

- ### 高性能的计算能力

  UXbot机器人配备高性能的Intel i5 处理器，8GB的内存以及超大的固态硬盘存储能力为您的应用开发提供十分强大的计算资源支持。

- ### ROS系统全支持

  UXbot机器人软件框架专为ROS系统定制，可运行ROS系统下的所有软件和算法，运动控制和规划算法完全支持ROS系统协议，为更多的学习和开发者提供通用的算法验证和应用落地的平台。

- ### 安全的机器运行保障

  为了保障机器人在运行过程中的安全性，UXbot配备了前后各3个倒车级的超声距离探测器，用于检测靠近机器人的物体和各种障碍物。当机器人靠近障碍物过近时候，机器人会在ROS状态消息中发出相应的报警消息，并且能够定位到哪个方向发出报警。

  另外，机器人配备前后各3个红外距离探测器，能够精准地探测到机器人下底板到地面的距离，当机器人悬空或者临近悬崖时会发出精确的报警。

## 三、产品参数

| 项目                   | 指标                                      |
| -------------------- | --------------------------------------- |
| 运动控制精度               | 0.05~1m/s                               |
| 雷达扫描频率               | 10Hz                                    |
| 雷达数据点数量              | 4000/s                                  |
| 雷达FOV（Field Of View） | 360度，200度可用                             |
| 工控机处理器               | Intel i5                                |
| 工控机内存                | 8GB                                     |
| 工控机存储容量              | 128GB SSD                               |
| 电池容量                 | 24V/10AH*2                              |
| 标准续航时长               | 10h                                     |
| 红外测距                 | 10~80cm*6                               |
| 超声测距                 | 20~600cm*6                              |
| 交互平板                 | Android                                 |
| 人体工学云台               | 水平-90~90度，俯仰-45～45度                     |
| 视觉模组                 | 3.5m，深度图像60fps@640x480，RGB图像30fps@1080p |
| 网络                   | 百兆网络机器人路由，支持2G、3G、4G网络                  |

## 四、[机器人选配表](https://github.com/yowlings/xbot2_info/blob/master/UXbot-select.xlsx?raw=true)

| 组件            | 型号             | 价格    | 主要参数                                     |
| :------------ | -------------- | ----- | ---------------------------------------- |
| 机器人底盘（必配）     | xbot-base      |       |                                          |
| 激光雷达（选配，可选型号） | rplidarA2-8m   | 2600  | 360度，8m                                  |
|               | sick561-10m    | 12000 | 270度，10m                                 |
|               | sick571-25m    | 21000 | 270度，25m                                 |
| 升降台(必配，可选型号）  | xbot-support   |       | 支撑平台                                     |
|               | xbot-lift      |       | 可升降                                      |
| 两自由度云台（必配）    | xbot-cloud     | 300   | 水平旋转180度，俯仰90度                           |
| 视觉模组（选配，可选型号） | Realsense R200 | 1400  | 远距离（3米.4米，室外更远）景深/红外：每秒60帧时，分辨率640X480 RGB（红绿蓝）:每秒30帧时，1080P外观尺寸：130X20X7mm支持Wibdow8.1（64位）安卓支持即送将推出要求USB3.0 |
|               | Asus Xtion2    | 2000  | 30FPS、640 x 480高景深分辨率，以及500万像素镜头，不仅可提供开发者更精确细緻的高品质深度图及彩色影像，适用范围可从0.8m延伸至3.5m |
| 主控板（必配，可选型号）  | NUC i5         | 3500  |                                          |
|               | NUC i7         | 4200  |                                          |
|               | GR8 i7+GTX1060 | 8700  | 高性能处理器，独立显卡                              |
| 机械臂（选配）       | xbot-arm       | 30000 | 7自由度手臂                                   |

## 五、产品尺寸

| 项目      |        |
| ------- | ------ |
| 机器人最大高度 | 1400mm |
| 机器人最大宽度 | 550mm  |
| 机器人轮间距  | 490mm  |



## 六、机器人验收标准

| 编号   | 验收演示项目               | 标准效果               |
| :--- | :------------------- | :----------------- |
| 1    | Xbot ROS驱动程序节点启动     | 能显示ROS消息           |
| 2    | 激光雷达节点启动             | rplidar转动，雷达数据消息发送 |
| 3    | App控制急停开关            | 开启时电机可转，关闭时电机失效    |
| 4    | App遥控                | 可八方向遥控             |
| 5    | App控制云台水平旋转          | 水平云台转动             |
| 6    | App控制云台竖直俯仰          | 竖直云台俯仰             |
| 8    | App摄像头画面实时显示         | 摄像头画面实时回传，延迟小于0.5s |
| 9    | 硬件急停开关控制             | 按住急停电机失效           |
| 10   | 硬件电池状态监视面板           | 电池液晶面板显示           |
| 11   | HDMI、USB、外接电源12V、19V | 接口有用               |


