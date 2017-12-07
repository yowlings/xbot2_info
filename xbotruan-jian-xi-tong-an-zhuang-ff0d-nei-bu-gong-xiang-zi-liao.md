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

