## 产品介绍 {#A.2BTqdUwU7Lfs0--1}

![](http://wiki.ros.org/Robots/Xbot/tutorial/cn/Product%20Spec?action=AttachFile&do=get&target=xbot2_360.gif "Xbot2 360 rotation")

xbot系列机器人是重德智能科技出品的一款包含机器人硬件平台、软件平台和操作系统支持三方面在内的机器人平台解决方案。该机器人经历了xbot1与xbot2两代机器人的开发与改进，目前已完全支持室内环境中的所有的机器人相关传感器接入和机器人应用。该平台自带运动控制系统、二维激光雷达点云测距、超声测距、红外测距、升降平台控制器、高清摄像头（RGB、深度可选）以及机器人视觉伺服云台。另外，由于机器人搭载了高性能的处理器和N系列高性能显卡，支持cuda加速以及运行绝大部分运算需求较高的应用，并且对于ROS的完全支持使得机器人可以接入任何支持ROS框架的硬件传感器或者其他开源软件。xbot2机器人平台将是您室移动内机器人应用的不二选择。



## 产品特点 {#A.2BTqdUwXJ5cLk-}

**稳定、可靠的运动控制**

Xbot机器人运动电机采用先进的PID鲁棒性控制算法，提供十分稳定、可靠的机器人运动控制，配以高减速比的高精度电机，机器人运动速度可控制到0.01m/s的精度，最小速度达0.01m/s，最大速度超2m/s。具有加速时间短，制动效果明显等的多方面优秀特性。

**完备的驱动软件支持**

我们为Xbot机器人提供完备的驱动软件，采用国际通用的驱动软件框架和通信协议，能够提供50Hz频率以上的数据心跳包传输和快速精准地数据编码解码功能，使机器人的运动状态控制精度到达20ms以上，从而机器人能够更加迅速地相应用户算法的控制。

**自主建图定位与导航（可选）**

Xbot机器人具备室内环境下的自主建图定位与导航功能，该功能让机器人在室内实现完全自主的同步建图和定位，从而机器人能够根据用户需求，在任意位置之间自由穿梭行走，同时在导航过程中精准避障，全自主规划行走路径。

**超长续航与自主充电**

Xbot机器人配备高达60Ah的超大容量电池，续航时间最高可达15小时。同时支持用户预约返回充电和自主返回充电模式，机器人智能管理自身的能量，在能量不足时自动返回充电桩充电。

**高性能计算能力**

Xbot机器人配备高性能的CPU计算能力和超强的GPU计算能力，支持cuda加速，运行超大场景的openpose算法速度达3fps以上，为您的应用计算提供强大的支持。

**ROS系统全支持**

Xbot机器人软件框架专为ROS系统定制，可运行ROS系统下的所有软件和算法，运动控制和规划算法完全支持ROS系统协议，为更多的学习和开发者提供通用的算法验证和应用落地的平台。



## 性能指标 {#A.2BYCeA.2FWMHaAc-}

|  | 高配版 | 中配版 | 低配版 |
| :--- | :--- | :--- | :--- |
| ﻿可靠测距范围 | ﻿0.05~25m | ﻿0.05~25m | ﻿8m |
| ﻿扫描频率 | ﻿15HZ | ﻿15HZ | ﻿10Hz |
| ﻿数据点数量 | ﻿13000/s | ﻿13000/s | ﻿4000/s |
| ﻿FOV | ﻿270° | ﻿270° | ﻿360° |
| CPU | Intel Core i7-7700 | 2x丹佛+4xA57 | 2x丹佛+4xA57 |
| 内存 | 8GB | 8GB | 8GB |
| 显卡 | NVIDIA GTX1060 | Pascal | Pascal |
| 显存 | 3GB | 2GB | 2GB |
| SSD大小 | 512GB | 32GB+256GB | 32GB+128GB |
| 电池容量 | 24V/30AH\*2 | 24V/15AH\*2 | 24V/10AH\*2 |
| 续航时长 | 12h | 15h | 10h |
| 红外测距范围 | 4~30cm\*3 | 4~30cm\*3 | 10~80cm\*3 |
| 超声测距范围 | 20~600cm\*3 | 20~600cm\*3 | 20~600cm\*3 |
| 单臂自由度 | 6 | 4 | 4 |
| 触控平板 | Android | Android | Android |
| 视觉模组 | 高清RGBD | RGBD | 双目高清 |



  




