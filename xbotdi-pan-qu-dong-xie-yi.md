# １、底盘各轮子位置及移动控制方向指示图

![](/assets/import.png)![](/assets/double.png)**备注：**

**a：计算机发给底盘的指令中，对于双轮底盘，Linear\_y\_speed方向的速度指令值无效，因为双轮不能平移。**

**b:底盘反馈给计算机的指令中，对于双轮底盘，电流、码盘数只有Front\_left、Front\_right数据有效，Back\_left、Back\_right数据全0，无效。**

# 2协议说明

综述：

底盘与计算机通过预先设定的协议进行通信。通常，计算机会发送指令给底盘，并且得到底盘的反馈数据和传感器信息。这些命令和反馈数据被转换为bytestream通过串口通信，通信协议规定了bytestream的规则和形式。

**2.1** bytestream组成

一个bytestream可以分为四部分：headers, length, payload和checksum.

| header0 | header1 | length | [payload](#_数据域内容：) | checksum |
| :--- | :--- | :--- | :--- | :--- |
| 1 Byte | 1 Byte | 1 Byte | N Byte | 1Bytes |
| 0XAA | 0X55 | 有效信息字节数 | 有效信息见2.2 | 除帧头外所有数据的异或 |

header

header包含两个字节，header0和header1，它们是底盘驱动命令中的固定值，headers用来检测一帧命令的开始，相当于起始位。

2、length

length表示一个payload的长度。

3、payload

payload包含bytestream中的实际数据即有效数据。

4、checksum

checksum是整个bytestream中除了headers外的异或值，checksum过程确保了bytestream的完整性。

5、序列化—反序列化

序列化是将数据结构转化为bytestream的过程，反序列化是一个逆转过程。每个数据类型都通过[LSB](http://en.wikipedia.org/wiki/Least_significant_byte)-Firstorder序列化和反序列化。这就意味着有效字节中的最低位将最先进入bytestream。例如，整形数据2,864,434,397（0xAABBCCDD）序列化后是：

| 0xDD | 0xCC | 0xBB | 0xAA |
| :--- | :--- | :--- | :--- |


所以，0xDD是最先进入bytestream的。

**2.2** payload由命令标识符和命令内容构成。

### 2.2.1计算机发送给底盘的命令中payload的内容：

| 命令标识符 | 命令内容 | 字节数 | 字节编号 | 数据定义 | 说明 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0x01 | PowerEnable | 1 | 1 | u 8 | 0x00:失能电源；0x01：使能电源 |
| 0x03\(note1\) | Linear\_x\_speed | 4 | 1-4 | float | Liner\_x\_speed：单位m/sLiner\_y\_speed：单位m/sangle\_speed：单位度/s |
|  |  | Linear\_y\_speed | 4 | 5-8 | float |
|  |  | angle\_speed | 4 | 9-12 | float |
| 04 | lift | 1 | 1 | u 8 | 0x01 : enable; 0x00: disable; |
|  | location | 1 | 2 | u8 | location: 0~100，步进值1 |
| 05 | Cloud steering | 1 | 1 | u 8 | Cloud steering：位置\(0-180\) |
|  | Camera steering | 1 | 2 | u8 | Camera steering:位置（0-180） |

note1：请以大于5hz小于10hz的频率循环发送此速度控制指令，驱动板会以此作为心跳包，如果超过500ms未接收到此指令，驱动板会自动将底盘速度复位为0。Angle\_speed向左转为正值,向右转为负值.

### 2.2.2底盘上传给计算机传感器数据中payload的内容：

| 命令标识符 | 命令内容 | 字节数 | 字节编号 | 数据定义 | 说明 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0x10 | Fixed | 1 | 1 | u8 | 0x01 |
|  | battery voltage | 2 | 2-3 | u16 | （备注1） |
|  | Fixed | 1 | 4 | u8 | 0x03 |
|  | Rear\_left红外测距值 | 2 | 5-6 | u16 | （备注2） |
|  | Rear\_cent红外测距值 | 2 | 7-8 | u16 | （备注2） |
|  | Rear\_right红外测距值 | 2 | 9-10 | u16 | （备注2） |
|  | Fixed | 1 | 11 | u8 | 0x05 |
|  | Front\_left电机电流值 | 2 | 12-13 | u16 | （备注3） |
|  | Front\_right电机电流值 | 2 | 14-15 | u16 | （备注3） |
|  | Rear\_left电机电流值 | 2 | 16-17 | u16 | （备注3） |
|  | Rear\_right电机电流值 | 2 | 18-19 | u16 | （备注3） |
|  | Up\_down电机电流值 | 2 | 20-21 | u16 | （备注3） |
|  | Fixed | 1 | 22 | u8 | 0x03 |
|  | Front\_left超声测距 | 2 | 23-24 | u16 | （备注4） |
|  | Front\_cent超声测距 | 2 | 25-26 | u16 | （备注4） |
|  | Front\_right超声测距 | 2 | 27-28 | u16 | （备注4） |
|  | Fixed | 1 | 29 | u8 | 0x05 |
|  | Front\_left电机码盘数 | 2 | 30-31 | u16 | （备注5） |
|  | Front\_right电机码盘数 | 2 | 32-33 | u16 | （备注5） |
|  | Back\_right电机码盘数 | 2 | 34-35 | u16 | （备注5） |
|  | Back\_left电机码盘数 | 2 | 36-37 | u16 | （备注5） |
|  | 升降电机码盘数 | 2 | 38-39 | u16 | （备注5） |
|  | Fixed | 1 | 40 | u8 | 0x01 |
|  | acce\_x | 2 | 41-42 | s16 | X轴加速度计大小（备注6） |
|  | acce\_y | 2 | 43-44 | s16 | Y轴加速度计大小（备注6） |
|  | acce\_z | 2 | 45-46 | s16 | Z轴加速度计大小（备注6） |
|  | gyro\_x | 2 | 47-48 | s16 | X轴角速度计大小（备注7） |
|  | gyro\_y | 2 | 49-50 | s16 | Y轴角速度计大小（备注7） |
|  | gyro\_z | 2 | 51-52 | s16 | Z轴角速度计大小（备注7） |
|  | mag\_x | 2 | 53-54 | s16 | X轴磁强计计大小（备注8） |
|  | mag\_y | 2 | 55-56 | s16 | Y轴磁强计计大小（备注8） |
|  | mag\_z | 2 | 57-58 | s16 | Z轴磁强计计大小（备注8） |
|  | Temp | 4 | 59-62 | float | 温度值（单位℃） |
|  | Yaw | 2 | 63-64 | s16 | 航向角（备注9） |
|  | Pitch | 2 | 65-66 | s16 | 俯仰角（备注9） |
|  | Roll | 2 | 67-68 | s16 | 翻滚角（备注9） |
|  | 时间戳 | 2 | 69-70 | u16 | 0～65535循环单位1us |

备注1：电池电压（V）占2个字节拼成u16格式后，假如[V](http://www.baidu.com/link?url=6Pdrj_6G1ikUrv6rbsDdsLkuusz0q6mGgFLAX8A01LTWh2yJ8Sw_ZcBsTNO1IY-h5Q5WtRYqFNadRGGwbVkjMaD8l3HjsNDzJPGLpHivL5G75uCESBIJ4IJf5uIX1Lmi)ﾧ=0x2596，表示电池电压25.96V, 假如[V](http://www.baidu.com/link?url=6Pdrj_6G1ikUrv6rbsDdsLkuusz0q6mGgFLAX8A01LTWh2yJ8Sw_ZcBsTNO1IY-h5Q5WtRYqFNadRGGwbVkjMaD8l3HjsNDzJPGLpHivL5G75uCESBIJ4IJf5uIX1Lmi)ﾧ=0x2712，表示电池电压27.12V,

备注2：红外测距值（D）占2个字节，拼成u16后，放大10倍，单位cm。

备注3：电机电流（I）占2个字节拼成u16格式后，放大10倍，单位A。

备注4：超声测距（D）占2个字节拼成u16格式后，放大10倍，单位cm。

备注5：电机码盘计数，占2个字节，拼成u16格式，码盘计数为0~65535循环计数，轮子转一周码盘计数增量8000。

备注6：加速度计X/Y/Z轴加速度大小，各占2个字节，拼成s16类型，数据大小范围-32864~ +32864 ，

1 LSB=0.00006086g，-32864表示加速度大小为-2g，+32864表示加速度大小为+2g。

备注7：陀螺仪X/Y/Z轴角速度大小，各占2个字节，拼成s16类型，数据范围大小-32864~+32864，

1 LSB=0.0152139846947314度，-32864表示角速度-500°/s，+32864表示角速度500°/s.

备注8：磁强计X/Y/Z轴测得地磁场强度，各占2个字节，拼成s16类型，数据大小范围-2048~+2047，

1 LSB=0.0004296875Gauss

备注9：姿态角yaw/pitch/roll，各占2个字节，拼成s16类型，输出的结果yaw/pitch/roll都放大了10倍。

### 2.2.3底盘上传给计算机的GPS数据

该数据遵循NMEA0813标准协议：NMEA 0183是美国国家海洋电子协会（National Marine Electronics Association）为海用电子设备制定的标准格式。目前业已成了GPS导航设备统一的RTCM（Radio Technical Commission for Maritime services）标准协议。

| 序号 | 命令 | 说明 | 最大帧长 |
| :--- | :--- | :--- | :--- |
| 1 | $GPGGA | 全球定位数据 | 72 |
| 2 | $GPGSA | 卫星PRN数据 | 65 |
| 3 | $GPGSV | 卫星状态信息 | 210 |
| 4 | $GPRMC | 运输定位数据 | 70 |
| 5 | $GPVTG | 地面速度信息 | 34 |
| 6 | $GPGLL | 大地坐标信息 |  |
| 7 | $GPZDA | UTC时间和日期 |  |

注：发送次序$PZDA、$GPGGA、$GPGLL、$GPVTG、$GPGSA、$GPGSV\*3、$GPRMC

协议帧总说明：

该协议采用ASCII码，其串行通信默认参数为：波特率=4800bps，数据位=8bit，开始位=1bit，停止位=1bit，无奇偶校验。

帧格式形如：$aaccc,ddd,ddd,…,ddd\*hh&lt;CR&gt;&lt;LF&gt;

1、“$”——帧命令起始位

2、aaccc——地址域，前两位为识别符，后三位为语句名

3、ddd…ddd——数据

4、“\*”——校验和前缀

5、hh——校验和（check sum），$与\*之间所有字符ASCII码的校验和（各字节做异或运算，得到校验和后，再转换16进制格式的ASCII字符。）

6、&lt;CR&gt;&lt;LF&gt;——CR（Carriage Return）+ LF（Line Feed）帧结束，回车和换行

GPGGA

GPS固定数据输出语句，这是一帧GPS定位的主要数据，也是使用最广的数据。

$GPGGA,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;,&lt;9&gt;,&lt;10&gt;,&lt;11&gt;,&lt;12&gt;,&lt;13&gt;,&lt;14&gt;\*&lt;15&gt;&lt;CR&gt;&lt;LF&gt;

&lt;1&gt; UTC时间，格式为hhmmss.sss。

&lt;2&gt;纬度，格式为ddmm.mmmm（前导位数不足则补0）。

&lt;3&gt;纬度半球，N或S（北纬或南纬）。

&lt;4&gt;经度，格式为dddmm.mmmm（前导位数不足则补0）。

&lt;5&gt;经度半球，E或W（东经或西经）。

&lt;6&gt;定位质量指示，0=定位无效，1=定位有效。

&lt;7&gt;使用卫星数量，从00到12（前导位数不足则补0）。

&lt;8&gt;水平精确度，0.5到99.9。

&lt;9&gt;天线离海平面的高度，-9999.9到9999.9米

&lt;10&gt;高度单位，M表示单位米。

&lt;11&gt;大地椭球面相对海平面的高度（-999.9到9999.9）。

&lt;12&gt;高度单位，M表示单位米。

&lt;13&gt;差分GPS数据期限（RTCM SC-104），最后设立RTCM传送的秒数量。

&lt;14&gt;差分参考基站标号，从0000到1023（前导位数不足则补0）。

&lt;15&gt;校验和。

GPGSA

GPS精度指针及使用卫星格式

$GPGSA,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;,&lt;9&gt;,&lt;10&gt;,&lt;11&gt;,&lt;12&gt;,&lt;13&gt;,&lt;14&gt;,&lt;15&gt;,&lt;16&gt;,&lt;17&gt;\*&lt;18&gt;&lt;CR&gt;&lt;LF&gt;

&lt;1&gt;模式2：M =手动，A =自动。

&lt;2&gt;模式1：定位型式1 =未定位，2 =二维定位，3 =三维定位。

&lt;3&gt;第1信道正在使用的卫星PRN码编号（Pseudo Random Noise，伪随机噪声码），01至32（前导位数不足则补0，最多可接收12颗卫星信息）。

&lt;4&gt;第2信道正在使用的卫星PRN码编号

&lt;5&gt;第3信道正在使用的卫星PRN码编号

&lt;6&gt;第4信道正在使用的卫星PRN码编号

&lt;7&gt;第5信道正在使用的卫星PRN码编号

&lt;8&gt;第6信道正在使用的卫星PRN码编号

&lt;9&gt;第7信道正在使用的卫星PRN码编号

&lt;10&gt;第8信道正在使用的卫星PRN码编号

&lt;11&gt;第9信道正在使用的卫星PRN码编号

&lt;12&gt;第10信道正在使用的卫星PRN码编号

&lt;13&gt;第11信道正在使用的卫星PRN码编号

&lt;14&gt;第12信道正在使用的卫星PRN码编号

&lt;15&gt; PDOP综合位置精度因子（0.5 - 99.9）

&lt;16&gt; HDOP水平精度因子（0.5 - 99.9）

&lt;17&gt; VDOP垂直精度因子（0.5 - 99.9）

&lt;18&gt;校验和

GPGSV

可视卫星状态输出语句

$GPGSV, &lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,...,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;\*&lt;8&gt;&lt;CR&gt;&lt;LF&gt;

&lt;1&gt;总的GSV语句电文数。

&lt;2&gt;当前GSV语句号。

&lt;3&gt;可视卫星总数，00至12。

&lt;4&gt;卫星编号，01至32。

&lt;5&gt;卫星仰角，00至90度。

&lt;6&gt;卫星方位角，000至359度。实际值。

&lt;7&gt;信噪比（C/No），00至99dB；无表未接收到讯号。

&lt;8&gt;校验和。

注：每条语句最多包括四颗卫星的信息，每颗卫星的信息有四个数据项，即：卫星编号、卫星仰角、卫星方位角、信噪比。

GPRMC

推荐最小数据量的GPS信息（Recommended Minimum Specific GPS/TRANSIT Data）

$GPRMC,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;,&lt;9&gt;,&lt;10&gt;,&lt;11&gt;,&lt;12&gt;\*&lt;13&gt;&lt;CR&gt;&lt;LF&gt;

&lt;1&gt; UTC（Coordinated Universal Time）时间，hhmmss（时分秒）格式

&lt;2&gt;定位状态，A=有效定位，V=无效定位

&lt;3&gt; Latitude，纬度ddmm.mmmm（度分）格式（前导位数不足则补0）

&lt;4&gt;纬度半球N（北半球）或S（南半球）

&lt;5&gt; Longitude，经度dddmm.mmmm（度分）格式（前导位数不足则补0）

&lt;6&gt;经度半球E（东经）或W（西经）

&lt;7&gt;地面速率（000.0~999.9节，Knot，前导位数不足则补0）

&lt;8&gt;地面航向（000.0~359.9度，以真北为参考基准，前导位数不足则补0）

&lt;9&gt; UTC日期，ddmmyy（日月年）格式

&lt;10&gt; Magnetic Variation，磁偏角（000.0~180.0度，前导位数不足则补0）

&lt;11&gt; Declination，磁偏角方向，E（东）或W（西）

&lt;12&gt; Mode Indicator，模式指示（仅NMEA0183 3.00版本输出，A=自主定位，D=差分，E=估算，N=数据无效）

&lt;13&gt;校验和。

GPVTG

地面速度信息

$GPVTG,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;,&lt;9&gt;\*&lt;10&gt;

&lt;1&gt;真北参照系运动角度（000到359度，前导位数不足则补0）。

&lt;2&gt;运动角度参照系，&lt;&gt;

nmea数据如下：

$GPGGA,121252.000,3937.3032,N,11611.6046,E,1,05,2.0,45.9,M,-5.7,M,,0000\*77$GPRMC,121252.000,A,3958.3032,N,11629.6046,E,15.15,359.95,070306,,,A\*54$GPVTG,359.95,T,,M,15.15,N,28.0,K,A\*04$GPGGA,121253.000,3937.3090,N,11611.6057,E,1,06,1.2,44.6,M,-5.7,M,,0000\*72$GPGSA,A,3,14,15,05,22,18,26,,,,,,,2.1,1.2,1.7\*3D$GPGSV,3,1,10,18,84,067,23,09,67,067,27,22,49,312,28,15,47,231,30\*70$GPGSV,3,2,10,21,32,199,23,14,25,272,24,05,21,140,32,26,14,070,20\*7E$GPGSV,3,3,10,29,07,074,,30,07,163,28\*7D说明：NMEA0183格式以“$”开始，主要语句有GPGGA，GPVTG，GPRMC等

1、GPS DOP and Active Satellites（GSA）当前卫星信息

$GPGSA,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;3&gt;,,,,,&lt;3&gt;,&lt;3&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;&lt;cr&gt;&lt;/cr&gt;&lt;lf&gt;&lt;/lf&gt;&lt;1&gt;模式：M =手动，A =自动。&lt;2&gt;定位型式1 =未定位，2 =二维定位，3 =三维定位。&lt;3&gt;PRN数字：01至32表天空使用中的卫星编号，最多可接收12颗卫星信息。

&lt;4&gt; PDOP位置精度因子（0.5~99.9）

&lt;5&gt; HDOP水平精度因子（0.5~99.9）

&lt;6&gt; VDOP垂直精度因子（0.5~99.9）

&lt;7&gt; Checksum.\(检查位\). 2、GPS Satellites in View（GSV）可见卫星信息

$GPGSV, &lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,?&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;&lt;cr&gt;&lt;/cr&gt;&lt;lf&gt;&lt;/lf&gt;&lt;1&gt; GSV语句的总数

&lt;2&gt;本句GSV的编号

&lt;3&gt;可见卫星的总数，00至12。

&lt;4&gt;卫星编号，01至32。&lt;5&gt;卫星仰角，00至90度。&lt;6&gt;卫星方位角，000至359度。实际值。&lt;7&gt;讯号噪声比（C/No），00至99 dB；无表未接收到讯号。&lt;8&gt;Checksum.\(检查位\).第&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;项个别卫星会重复出现，每行最多有四颗卫星。其余卫星信息会于次一行出现，若未使用，这些字段会空白。3、Global Positioning System Fix Data（GGA）GPS定位信息

$GPGGA,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;,&lt;9&gt;,M,&lt;10&gt;,M,&lt;11&gt;,&lt;12&gt;\*hh&lt;cr&gt;&lt;/cr&gt;&lt;lf&gt;&lt;/lf&gt;&lt;1&gt; UTC时间，hhmmss（时分秒）格式

&lt;2&gt;纬度ddmm.mmmm（度分）格式（前面的0也将被传输）

&lt;3&gt;纬度半球N（北半球）或S（南半球）

&lt;4&gt;经度dddmm.mmmm（度分）格式（前面的0也将被传输）

&lt;5&gt;经度半球E（东经）或W（西经）

&lt;6&gt; GPS状态：0=未定位，1=非差分定位，2=差分定位，6=正在估算

&lt;7&gt;正在使用解算位置的卫星数量（00~12）（前面的0也将被传输）

&lt;8&gt; HDOP水平精度因子（0.5~99.9）

&lt;9&gt;海拔高度（-9999.9~99999.9）

&lt;10&gt;地球椭球面相对大地水准面的高度

&lt;11&gt;差分时间（从最近一次接收到差分信号开始的秒数，如果不是差分定位将为空）

&lt;12&gt;差分站ID号0000~1023（前面的0也将被传输，如果不是差分定位将为空）

4、Recommended Minimum Specific GPS/TRANSIT Data（RMC）推荐定位信息

$GPRMC,&lt;1&gt;,&lt;2&gt;,&lt;3&gt;,&lt;4&gt;,&lt;5&gt;,&lt;6&gt;,&lt;7&gt;,&lt;8&gt;,&lt;9&gt;,&lt;10&gt;,&lt;11&gt;,&lt;12&gt;\*hh&lt;cr&gt;&lt;/cr&gt;&lt;lf&gt;&lt;/lf&gt;&lt;1&gt; UTC时间，hhmmss（时分秒）格式

&lt;2&gt;定位状态，A=有效定位，V=无效定位

&lt;3&gt;纬度ddmm.mmmm（度分）格式（前面的0也将被传输）

&lt;4&gt;纬度半球N（北半球）或S（南半球）

&lt;5&gt;经度dddmm.mmmm（度分）格式（前面的0也将被传输）

&lt;6&gt;经度半球E（东经）或W（西经）

&lt;7&gt;地面速率（000.0~999.9节，前面的0也将被传输）

&lt;8&gt;地面航向（000.0~359.9度，以真北为参考基准，前面的0也将被传输）

&lt;9&gt; UTC日期，ddmmyy（日月年）格式

&lt;10&gt;磁偏角（000.0~180.0度，前面的0也将被传输）

&lt;11&gt;磁偏角方向，E（东）或W（西）

&lt;12&gt;模式指示（仅NMEA0183 3.00版本输出，A=自主定位，D=差分，E=估算，N=数据无效）

5、Track Made Good and Ground Speed（VTG）地面速度信息

$GPVTG,&lt;1&gt;,T,&lt;2&gt;,M,&lt;3&gt;,N,&lt;4&gt;,K,&lt;5&gt;\*hh&lt;cr&gt;&lt;/cr&gt;&lt;lf&gt;&lt;/lf&gt;&lt;1&gt;以真北为参考基准的地面航向（000~359度，前面的0也将被传输）

&lt;2&gt;以磁北为参考基准的地面航向（000~359度，前面的0也将被传输）

&lt;3&gt;地面速率（000.0~999.9节，前面的0也将被传输）

&lt;4&gt;地面速率（0000.0~1851.8公里/小时，前面的0也将被传输）

&lt;5&gt;模式指示（仅NMEA0183 3.00版本输出，A=自主定位，D=差分，E=估算，N=数据无效）

附录：

**以下为计算机发送给底盘的命令举例：**

打开电源：AA 55 02 01 01 02

关闭电源：AA 55 02 01 00 03

前进速度0.2m/s:AA 55 0D 03CD CC 4C 3E00 00 00 00 00 00 00 00 7D

后退速度0.2m/s:AA 55 0D 03CD CC 4C BE00 00 00 00 00 00 00 00 FD

右平移速度0.3m /s：AA 55 0D 03 00 00 00 00CD CC 4C 3E00 00 00 00 7D

左平移速度0.3m /s：AA 55 0D 03 00 00 00 00CD CC 4C BE00 00 00 00 FD

右转向20°/s：AA 55 0D 03 00 00 00 00 00 00 00 0000 00 A0 41EF

左转向20°/s：AA 55 0D 03 00 00 00 00 00 00 00 0000 00 A0 C16F

gift命令：把升降平台最底部标为0，最顶部标为100，中间递增等间距标1~99位置值，升降到指定位置时只需发送对应的0~100位置值即可。

最低部0位置：AA 55 03 04 010006

最顶部100位置：AA 55 03 04 016462

10位置：AA 55 03 04 010A0C

云台：云台由一个180°舵机控制，云台0°时转向底盘正左方，云台180°时转向底盘正右方，云台90°时转向底盘正前方，开机上电，云台处于90°位置。

深度摄像头：深度摄像头由一个180°舵机控制，深度摄像头0°时，深度摄像头转向地，深度摄像头90°时转向底盘正前方，深度摄像头180°时转向天。开机上电，深度摄像头转向正前方。

云台角度：0°摄像头：90°AA 55 03 05005A5C

云台角度：90摄像头：0°AA 55 03 055A005C

云台角度：180°摄像头：180度AA 55 03 05B4B406

**以下为底盘上传给计算机的传感器数据：**

传感器数据：

例子：AA 55 47 10 01 62 26 03 02 00 01 00 00 00 05 0A 00 06 00 00 00 00 00 06 00 03 00 00 00 00 00 00 05 00 00 00 00 00 00 00 00 E8 03 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 02 DC 2F



