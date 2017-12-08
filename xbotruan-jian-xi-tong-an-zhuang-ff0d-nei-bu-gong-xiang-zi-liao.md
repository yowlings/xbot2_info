# 基于TX2主机的Xbot软件系统安装指导

## 一、TX2系统初始化

TX2原生自带了ubuntu系统的镜像文件，第一次启动TX2时，屏幕会显示系统初始化安装指导的命令行界面，上面标明了操作的详细步骤，按照其指引完成即可。

> 用户名：nvidia
>
> 密码：nvidia

进入NVIDIA\_INSTALLER文件夹，运行目录下的bash文件进行安装即可。

```bash
sudo ./installer.sh
```

等待程序运行完毕之后，重启即可进入ubuntu的系统界面。

```bash
sudo reboot
```

## 二、安装基础工具软件

### １、安装firefox浏览器

```bash
sudo apt-get update
sudo apt-get install firefox
```

安装完成后即可在菜单栏当中找到firefox浏览器的图标，有些情况下找不到时可重启一下即可。

### ２、安装git

```bash
sudo apt-get install git
```

## 三、安装编译好的TX2内核

由于自带的ubuntu系统不支持USB串口转换接口，亦即在运行指令

```
lsusb
```

之后虽然能够看到TX2当前连接的usb设备信息，但是在运行

```
ls /dev
```

之后并没有ttyUSB0这一项，即不支持USB串口转换。

而支持USB串口转换与否与ubuntu的系统内核的模块驱动有直接关系，亦即在内核中并没有勾选

> **devices support/usb convert support/FTDI devices**

这一项，因而需要对TX2的内核进行重新编译并勾选该选项。再此，我们已经将编译好的内核文件打包好了，只需要将两个文件夹boot/与modules/分别替换到当前系统的相应位置即可，运行

```
sudo cp -rf boot/ /
sudo cp -rf modules/ /lib
```

重启即可，重启后你将看到在ls /dev之后出现了ttyUSB0这一项。

## 四、对机器人端口进行映射

由于机器人的驱动程序以及ROS驱动包读取的是映射之后名为/dev/xbot的串口端口，所以我们需要将插上的控制板串口编号映射到该名称下，具体做法如下：

### 1、查看机器人控制板串口编号

运行指令

```
lsusb
```

查看对应的串口号，此处的端口号ID为类似于0403:6001的端口号，例如下图

![](/assets/port_map.png)

### 2、拷贝端口映射文件

将文件夹中的58-xbot.rules文件拷贝到/etc/udev/rules.d/文件夹中

```
sudo cp 58-xbot.rules /etc/udev/rules.d/
```

编辑拷贝完成的文件，然后更改内容当中的idVendor以及idProduct的编号为刚才第一步查到的编号。

```
sudo vi /etc/udev/rules.d/58-xbot.rules
```

### 3、重启端口管理服务

```
sudo service udev restart
```

或者

```
sudo /etc/init.d/udev restart
```

然后拔出串口再重新插上，如果ls /dev之后还是没有xbot这一项，如果没有则重新启动。

## 五、在TX2上安装ROS

具体安装过程可参照Nvidia jetson的官方github包，按照readme当中的指示完成即可。

[https://github.com/jetsonhacks/installROSTX2](https://github.com/jetsonhacks/installROSTX2)

```
git clone https://github.com/jetsonhacks/installROSTX2.git
cd installROSTX2
./updateRepositories.sh
./installROS.sh
./setupCatkinWorkspace.sh
```

## 六、安装xbot驱动程序以及ROS驱动包

进入重德智能科技的官方github页面[https://github.com/DroidAITech](https://github.com/DroidAITech)，获取相应的官方支持驱动包。

```
cd ~/catkin_ws/src/
git clone https://github.com/DroidAITech/xbot2
git clone https://github.com/DroidAITech/xbot2_description
```

## 七、安装xbot ROS驱动包依赖的软件

```
sudo apt-get install ros-kinect-controller-mannager
sudo apt-get install ros-kinect-gazebo-ros
sudo apt-get install ros-kinect-xacro
sudo apt-get install ros-kinect-rqt-plot
sudo apt-get install ros-kinect-rviz
```

## 八、编译运行xbot ROS驱动包

编译

```
cd catkin_ws
catkin_make
```

运行

```
roslaunch xbot_bringup xbot.launch
```

测试是否运行成功

```
rostopic echo /mobile_base/xbot/state
```

查看机器人状态消息。



























