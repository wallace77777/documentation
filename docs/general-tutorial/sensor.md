---
sidebar_label: "科易传感器"
sidebar_position: 10
---

## 开发环境搭建

该教程适用于瑞莎大部分 SBC 产品，此处使用 ROCK 4C+ 进行实际演示，其他 SBC 可参考此操作。

```bash
// 下载示例代码
rock@rock-4c-plus:~$ sudo apt-get install cmake build-essential git python3-dev -y
rock@rock-4c-plus:~$ git clone https://github.com/nascs/sample_code.git
rock@rock-4c-plus:~$ source sample_code/env.sh

// 安装 wiringX 库
rock@rock-4c-plus:~$ git clone https://github.com/wiringX/wiringX.git
rock@rock-4c-plus:~$ cd wiringX
rock@rock-4c-plus:~/wiringX/$ mkdir build
rock@rock-4c-plus:~/wiringX/$ cd build
rock@rock-4c-plus:~/wiringX/build$ cmake ..
rock@rock-4c-plus:~/wiringX/build$ make -j4
rock@rock-4c-plus:~/wiringX/build$ cpack -G DEB
rock@rock-4c-plus:~/wiringX/build$ sudo dpkg -i libwiringx*.deb
```

## LCD 1602/2004

keyestudio 1602 I2C 模块是一个 16 个字符的 2 行 LCD 显示器，采用蓝色背景和白色背光。 keyestudio 2004 I2C 模块是一个 20 个字符 4 行液晶显示屏，蓝色背景，白色背光。下面是在 radxa 平台上的使用方法:

1. 使用 rsetup 工具打开 i2c7

2. 断电关机

3. 将 LCD 按以下方式接线

```

LCD <--> Radxa ROCK 4
GND <--> GND
VCC <--> 5V
SDA <--> Pin 3
SCL <--> Pin 5

```

4. 重启，并检查 i2c7 是否开启

```bash
radxa@rock-4c-plus:~$ ls /dev/i2c-*
/dev/i2c-0  /dev/i2c-7  /dev/i2c-9 # 开启后可检测到 /dev/i2c-7
```

5. 通过以下命令检查 LCD 是否正常被识别

```bash
radxa@rock-4c-plus:~$ sudo i2cdetect -r -y 7
	 0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:						 -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- 27 -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

6. 运行 LCD 测试程序

```bash
sudo python LCD1602.py
```

该程序会显示系统当前时间。

## ADXL345

ADXL345 是一款小巧、轻薄、低功耗的三轴 MEMS 加速计，具有高分辨率（13 位）测量功能，最大 +-16 g。数字输出数据格式为 16 位二进制，可通过 SPI（3 线制或 4 线制）或 I2C 数字接口访问。ADXL345 非常适合测量倾斜感应应用中的静态重力加速度，以及运动或冲击产生的动态加速度。其高分辨率（4 mg/LSB）可测量小于 1.0 度的倾角变化。下面是在 radxa 平台上的使用方法:

1. 使用 rsetup 工具打开 i2c7

2. 断电关机

3. 将 sensor 按以下方式接线

```

ADXL345 <--> Radxa ROCK 4
GND <--> GND
VCC <--> 5V
SDA <--> Pin 3
SCL <--> Pin 5

```

4. 重启，并检查 i2c7 是否开启

```bash
radxa@rock-4c-plus:~$ ls /dev/i2c-*
/dev/i2c-0  /dev/i2c-7  /dev/i2c-9
```

5. 运行测试程序

```bash
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# gcc adxl345.c -lwiringx
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# ./a.out
```

该程序会显示 x y z 三个方向上的加速度数据。

## Button + LED

这有一个按钮和一个发光二极管。二者皆可通过编程控制，可实现按下按钮对发光二极管的控制。下面是在 radxa 平台上的使用方法:

1. 将 sensor 按以下方式接线

```

led <--> Radxa ROCK 4
 s <--> Pin 3
 v <--> 3.3V/5V
 g <--> GND

button <--> Radxa ROCK 4
 s <--> Pin 5
 v <--> 3.3V/5V
 g <--> GND

```

2. 运行测试程序

```bash
radxa@rock-4c-plus:~$ gcc button_led.c -lwiringx
radxa@rock-4c-plus:~$ sudo ./a.out
```

该程序会显示 button 是否被按下，若 button 被按下，则 led 是灭的。

## Ultrasonic sensor

Keyestudio SR01 超声波传感器价格实惠，可检测超声波传感器与障碍物之间的距离。它采用 CS100A 芯片，兼容 3.3V 和 5V。其最大探测距离为 3 米，盲区小于 4 厘米。与蝙蝠的原理一样，超声波模块发出的是人体听不到的高频信号。如果遇到障碍物，它们就会返回。接收到返回的信息后，它将通过确定发射信号和接收信号的时间差来计算传感器与障碍物之间的距离。下面是在 radxa 平台上的使用方法:

1. 将 sensor 按以下方式接线

```

ultrasonic sensor <--> Radxa ROCK 4
trig <--> Pin 3
echo <--> Pin 5
v <--> 3.3V/5V
g <--> GND

```

2. 运行测试程序

```bash
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# gcc ultrasonic_sensor.c -lwiringx
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# ./a.out
```

该程序会输出超声波传感器测得的距离。

## 4-digit 7-segment Display

keyestudio 4 位 LED 显示器模块集成了一个 0.36" 4 位 7 段显示器共阳极，有 12 个引脚。它使用 TM1637 驱动芯片。
该模块有 4 个间距为 2.54 毫米的控制引脚，可通过跳线直接连接到微控制器。因此，控制矩阵非常简单，无需大量布线。可让微控制器通过信号接口控制 4 位 LED 段显示屏，大大节省了微控制器的 IO 引脚资源。模块上有两个 3 毫米的固定孔，方便安装到其他设备上。如果您一直在关注矩阵显示屏，但因其复杂性而犹豫不决，那么这款产品就是您一直在寻找的解决方案！下面是在 radxa 平台上的使用方法:

1. 将 sensor 按以下方式接线

```

4-digit 7-segment Display <--> Radxa ROCK 4
CLK <--> Pin 40
DIO <--> Pin 38
VCC <--> 3.3V/5V
GND <--> GND

```

2. 运行测试程序

```bash
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# gcc tm1637.c -lwiringx
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# ./a.out
```

该程序会显示数字 "1024"。

## OLED module

OLED 是有机发光二极管的简称。在微观层面上，OLED 显示屏是由有机发光二极管组成的矩阵，当它们发出能量时就会发光。旧的 LCD（液晶显示器）技术使用电子控制的偏振片来改变光线通过或不通过的方式。这就需要一个外部背光灯来照亮整个显示屏。这需要消耗大量能源，因为在显示屏打开时，必须为所有像素提供足够的光线。新的 OLED 技术只消耗每个像素的电能。因为每个像素都能产生自己的光，所以只有开启的像素才用电。这使得 OLED 技术非常高效；此外，与 LCD 相比，这类 OLED 的制造方式使其非常薄。下面是在 radxa 平台上的使用方法:

1. 使用 rsetup 工具打开 i2c7

2. 断电关机

3. 将 sensor 按以下方式接线

```

OLED <--> Radxa ROCK 4
 GND	<-->	GND
 VCC	<--> 5V
 SDA	<-->	Pin 3
 SCL	<-->	Pin 5

```

4. 重启，并检查 i2c7 是否开启

```bash
radxa@rock-4c-plus:~$ ls /dev/i2c-*
/dev/i2c-0  /dev/i2c-7  /dev/i2c-9
```

5. 检查 OLED 是否正常被识别

```bash
radxa@rock-4c-plus:~$ sudo i2cdetect -r -y 7
	 0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:						 -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- 3c -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

6. 运行测试程序

```bash
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# gcc oled.c -lwiringx
root@rock-4c-plus:/home/radxa/sample_code/modules/keyestudio# ./a.out
```

该程序会显示字符 "Radxa ROCK 4"。

## Buzzer/LED/IR transmitter

这里有一个蜂鸣器，一个led和一个红外发射器。三者均可以通过编程来进行控制。下面是在 radxa 平台上的使用方法:

1. 将 sensor 按以下方式接线

```

buzzer/led/ir transmitter <--> Radxa ROCK 4
s <--> Pin 3
v <--> 3.3V/5V
g <--> GND

```

2. 运行测试程序

```bash
radxa@rock-4c-plus:~ cd sample_code/wiringX
radxa@rock-4c-plus:~/sample_code/wiringX$ gcc blink.c -lwiringx
```

该程序会每隔一秒闪一次灯，蜂鸣器的话会每个一秒响一次。

## TCS34725

keyestudio TCS34725 传感器主要使用 TCS34725 色彩传感器芯片。它可以通过 I2C 通信接口与其他控制器通信。Keyestudio TCS34725 色彩传感器是一款低成本、高性价比的 RGB 全彩色彩识别传感器。该传感器可通过光学感应识别物体的表面颜色。传感器在强光照射下输出相应的 RGB 值，帮助还原颜色。此外，为了避免周围环境的干扰，提高准确度，我们还特别在传感器底部增加了一块红外光屏蔽板，使入射光的红外光谱元素降到最低，从而使色彩管理更加准确。在传感器底部可以看到 4 颗黄色高亮 LED，可以保证传感器在环境光较弱的情况下正常使用，真正实现了补光功能。该传感器具有高灵敏度、宽动态范围和红外遮光滤镜。它是一种理想的色敏元件解决方案。它广泛应用于 RGB LED 背光控制、固态照明、健康产品等领域。下面是在 radxa 平台上的使用方法:

1. 拉取测试代码

```bash
radxa@rock-4c-plus:~$ git clone https://github.com/nascs/TCS34725.git
radxa@rock-4c-plus:~$ cd TCS34725
radxa@rock-4c-plus:~/TCS34725$ git checkout -b test origin/test
```

2. 使用 rsetup 工具打开 i2c7

3. 断电关机

4. 将 sensor 按以下方式接线

```

TCS34725 <--> Radxa ROCK 4
 GND <--> GND
 VCC <--> 5V
 SDA <--> Pin 3
 SCL <--> Pin 5

```

5. 重启，并检查 i2c7 是否开启

```bash
radxa@rock-4c-plus:~$ ls /dev/i2c-*
/dev/i2c-0  /dev/i2c-7  /dev/i2c-9
```

6. 检查 TCS34725 是否被识别

```bash
radxa@rock-4c-plus:~$ sudo i2cdetect -r -y 7
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:						 -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- 29 -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

7. 运行测试程序

```bash
radxa@rock-4c-plus:~/TCS34725$ sudo python3 example.py
```

该程序需要在光线比较暗的情况下运行，会识别物体的表面颜色，并输出 RGB 值。

## DS3231

DS3231 集成了 TCXO 和晶体，是一款高性价比、高精度的 I2C 实时时钟。该器件带有电池输入，因此即使断开主电源，它仍能保持精确的计时。集成振荡器确保了器件的长期精度，并减少了元件数量。DS3231 提供商用和工业温度范围，支持 16 引脚小型外线封装（300mil）。模块本身可适应 3.3V 和 5V 系统，无需电平开关，非常方便！下面是在 radxa 平台上的使用方法:

1. 使用 rsetup 打开 ds3231 overlay

2. 将 sensor 按以下方式接线

```

DS3231 <--> Radxa ROCK 4
GND <--> GND
VCC <--> 5V
SDA <--> Pin 3
SCL <--> Pin 5

```

3. 重启，并检查 i2c7 是否开启

```bash
radxa@rock-4c-plus:~$ ls /dev/i2c-*
/dev/i2c-0  /dev/i2c-7  /dev/i2c-9
```

4. 检查 DS3231 是否被识别

```bash
radxa@rock-4c-plus:~$ sudo i2cdetect -r -y 7
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:						 -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- UU -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

5. 添加一个新的 RTC 设备

```bash
root@rock-4c-plus:/home/radxa# echo ds3231 0x68 | sudo tee  /sys/class/i2c-adapter/i2c-7/new_device
```

6. 检查新的 RTC 设备

```bash
root@rock-4c-plus:/home/radxa# ls /dev/rtc*
/dev/rtc  /dev/rtc0 /dev/rtc1
```

7. 从 RTC 模块中读取时间

```bash
root@rock-4c-plus:/home/radxa# hwclock -r -f /dev/rtc1
2000-01-01 00:01:40.083622+08:00
```

8. 设置系统时间

```bash
root@rock-4c-plus:/home/radxa# apt-get install ntp -y
root@rock-4c-plus:/home/radxa# sudo service ntp start

or
date -s "2023-05-09 15:16:35"
```

9. 写入时间到 RTC 模块

- 确保 linux 时间正确后，将时间写入 rtc 模块

```bash
root@rock-4c-plus:/home/radxa# hwclock -w -f /dev/rtc1
```

- 然后读取硬件 RTC 的时间，看看它是否正确

```bash
root@rock-4c-plus:/home/radxa# hwclock -r -f /dev/rtc1
2023-05-09 15:18:45.390726+08:00
```

- 将 Linux 系统时间设置为硬件 RTC 时间

```bash
root@rock-4c-plus:/home/radxa# hwclock -s -f /dev/rtc1
```

- 使用 timedatectl 查看所有时间信息

```bash
root@rock-4c-plus:/home/radxa# timedatectl
               Local time: Tue 2023-05-09 15:21:49 CST
           Universal time: Tue 2023-05-09 07:21:49 UTC
                 RTC time: Thu 1970-01-01 00:04:27
                Time zone: Asia/Shanghai (CST, +0800)
System clock synchronized: no
              NTP service: n/a
          RTC in local TZ: yes

Warning: The system is configured to read the RTC time in the local time zone.
         This mode cannot be fully supported. It will create various problems
         with time zone changes and daylight saving time adjustments. The RTC
         time is never updated, it relies on external facilities to maintain it.
         If at all possible, use RTC in UTC by calling
         'timedatectl set-local-rtc 0'.

```

10. 设置开机自启

```bash
root@rock-4c-plus:/home/radxa# touch /etc/rc.local
root@rock-4c-plus:/home/radxa# chmod 777 /etc/rc.local
root@rock-4c-plus:/home/radxa# cat /etc/rc.local
#! /bin/bash

echo ds3231 0x68 | sudo tee  /sys/class/i2c-adapter/i2c-3/new_device
sudo hwclock -s -f /dev/rtc1
```

## DS18B20

DS18B20 是一款数字温度传感器。它可用于量化环境温度测试。温度范围为 -55 ~ +125 ℃，固有温度分辨率为 0.5 ℃。它还支持多点网状网络。部署 DS18B20 可实现多点温度测量。它具有 9-12 位串行输出。下面是在 radxa 平台上的使用方法:

1. 使用 rsetup 工具打开 one wire

2. 断电关机

3. 按以下接线方式连接

```

DS18B20 <--> Radxa ROCK 4
s <--> Pin 37 (GPIO4_D6)
v <--> 3.3V/5V
g <--> GND

```

4. 重启，并检设备是否被识别

若接线正常，目录 /sys/bus/w1/devices/ 下会有一个 28 开头的设备

5. 运行测试程序

```bash
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ gcc ds18b20.c
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ sudo ./a.out
```

该程序会输出当前温度。

## Fan motor

keyestudio L9110 风扇控制模块采用 L9110 电机控制芯片。它可以控制电机的旋转方向，从而控制风扇。该模块设计有安装孔，可兼容伺服电机控制。该模块效率高，配合高质量的风扇，可轻松吹灭 20 厘米距离内的灯火。它是消防机器人开发中必不可少的部件。以下是它在 radxa 平台上的使用方法：

1. 按以下方式接线

```

fan <--> Radxa ROCK 4
INA <--> Pin 13
INB <--> Pin 14
VCC <--> 3.3V/5V
GND <--> GND

```

2. 运行测试程序

```bash
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ gcc fan_motor.c -lwiringx
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ sudo ./a.out
```

## IR Receiver

红外线被广泛应用于遥控领域。 有了这个红外接收器，如果您有合适的解码器，您的项目就能接收来自任何红外遥控器的指令。在我们的 radxa 平台上，我们适配了 Car Mp3 遥控器。以下是它在 radxa 平台上的使用方法：

1. 使用 rsetup 工具打开 IR receiver

2. 按以下方式接线

```

IR Receiver <--> Radxa ROCK 4
s <--> Pin 13
v <--> 3.3V/5V
g <--> GND

```

3. 安装测试软件，进行测试

```bash
radxa@rock-4c-plus:~$ sudo apt-get install evtest -y
radxa@rock-4c-plus:~$ sudo evtest
```

我们适配的是 "Car MP3" 遥控器， 按下遥控器的按键，终端上可以看到相应的信息。

## MG90s

舵机是一种位置（角度）伺服的驱动器，适用于那些需要角度不断变化并可以保持的控制系统。以下是它在 radxa 平台上的使用方法：

1. 按以下方式接线

```

MG90s <--> Radxa ROCK 4
s <--> Pin 13
v <--> 3.3V/5V
g <--> GND

```

2. 运行测试程序

```bash
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ gcc servo.c -lwiringx
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ sudo ./a.out 90		//90 is angle
```

该程序会使 MG90s 按照您设定的角度转动。

## WS28b20

WS2812B是一种数字可编程LED灯珠，也被称为Neopixel。它是基于单总线控制的灯珠，可以在一根数据线上串联多个WS2812B灯珠，每个灯珠内置一个控制 IC 和 RGB LED。通过向其中一个灯珠发送颜色数据，可以实现所有灯珠的同步控制，从而形成各种颜色和动态效果的LED照明系统。WS2812B灯珠具有体积小、功耗低、可编程性强等特点，广泛应用于灯光装饰、视觉艺术、智能家居等领域。以下是它在 radxa 平台上的使用方法：

1. 使用 rsetup 工具打开 spi1

2. 断电关机

3. 按以下方式接线

```

ws2812b <--> Radxa ROCK 4
IN <--> Pin 19
GDN <--> GND
VCC <--> VCC

```

5. 重启，检查 spi1 是否正常打开

6. 运行测试程序

```bash
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ sudo python3 ws2812b.py
```

该程序会逐个点亮 WS2812B 灯，然后再全部熄灭。

## DHT11/DHT22

DHT11/DHT22是一种数字式温湿度传感器。它采用了高精度温湿度传感器和专用的模数转换器，可将温度和湿度信息数字化后通过单总线进行传输。DHT11适用于室内环境测量，测量范围温度0-50℃，湿度 20-90RH%，精度为±2℃和±5%RH，DHT22适用于更为苛刻的环境测量，测量范围-40~80℃，湿度 0-100RH%，精度为±0.5℃和±2%RH。这两种传感器具有结构简单、使用方便、价格低廉等优点，在物联网、智能家居、环境监测等领域得到了广泛应用。以下是它在 radxa 平台上的使用方法：

1. 按以下方式接线

```

DHT11/DHT22 <--> Radxa ROCK 4
S <--> Pin 3
GDN <--> GND
VCC <--> VCC

```

2. 运行测试程序

```bash
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ gcc dht11.c -lwiringx
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$ ./a.out ROCK 4 8
```

该程序会输出 DHT 模块检测到的温度和湿度

## Bluetooth module

该蓝牙模块可轻松实现串行无线数据传输。它的工作频率属于最流行的 2.4GHz ISM 频段（即工业、科学和医疗频段）。它采用蓝牙 2.1+EDR 标准。在蓝牙 2.1 中，不同设备的信号发射时间间隔为 0.5 秒，这样就可以大大减少蓝牙芯片的工作量，为蓝牙节省更多的休眠时间。该模块采用串行接口，使用方便，简化了整体设计/开发周期。以下是它在 radxa 平台上的使用方法：

1. 使用 rsetup 工具打开 uart4

2. 断电关机

3. 按以下方式接线

```

HC-06 <--> Radxa ROCK 4
RXD <--> Pin 19
TXD <--> Pin 21
VCC <--> 3.3/5V
GND <--> GND

```

4. 检查 uart4 是否正常打开

```bash
radxa@rock-4c-plus:~$ ls
/dev/ttyS4
```

5. 安装 minicom 或者其他串口调试工具

```bash
radxa@rock-4c-plus:~$ sudo apt-get install minicom -y
```

6. 打开串口调试工具

```bash
sudo minicom -D /dev/ttyS4 -b 9600
```

7. 手机或者电脑安装蓝牙调试工具，并连接蓝牙模块

8. 测试数据收发

使用手机或者电脑的蓝牙调试工具发送数据，minicom 会收到来自手机或者电脑端发送的数据。

## Rotation Sensor

模拟旋转传感器的电压可细分为 1024。以下是它在 radxa 平台上的使用方法：

1. 按以下方式接线

```

rotation sensor <--> Radxa ROCK 4
s <--> Pin 26
v <--> 3.3/5V
g <--> GND

```

2. 运行测试程序

```bash
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$  gcc rotation_sensor.c
radxa@rock-4c-plus:~/sample_code/modules/keyestudio$  sudo ./a.out
IIO device value: 954
```

## RfID RC522

MF522-AN 模块采用飞利浦 MFRC522 原装读写器电路芯片设计，使用方便，成本低，适合设备开发、读写器开发等高级应用的用户，需要进行射频卡终端设计/生产的用户。该模块可直接装入各种读写器模具中。模块使用电压为 3.3V，可通过 SPI 接口使用简单的几条线直接连接到用户任意 CPU 板的通信模块上，从而保证读卡器距离稳定可靠。

1. 使用 rsetup 工具打开 spi1

2. 断电关机

3. 按以下方式接线

```

RfID RC522 <--> Radxa ROCK 4
vcc <--> 3.3V/5V
rst <--> Pin 36
gnd <--> GND
miso <--> Pin 21
mosi <--> Pin 19
sck <--> Pin 23
nss <--> Pin 38
irq <--> Pin 40

```

2. 重启，检查 spi1 是否正常打开

```bash
radxa@rock-4c-plus:~$ ls /dev/spidev*
/dev/spidev1.0
```

2. 安装必要库

```bash
sudo apt update
sudo apt install python-dev python3-dev
sudo pip3 install spidev
sudo pip3 install mfrc522
```

3. 修改库文件

将目录 /usr/local/lib/python3.9/dist-packages/mfrc522 下的 python 文件里的 RPI.GPIO 修改为 OPi.GPIO

4. 写卡测试

将空白门禁卡放在读卡器上，运行程序，这将写入一个字符串 "Hello, world"

5. 读卡测试

将空白门禁卡放在读卡器上，运行程序，如果读到卡 id 和 字符串 "Hello, world"，则说明程序正常工作。

## GPIO Shield

PCF8591 具有四个 8 位模数转换器和一个 8 位数模转换器。它将通过 Raspberry Pi 上的 I2C 接口工作。它还可以在 radxa 平台上使用，下面是它在 radxa 平台上的使用方法：

1. 使用 rsetup 工具打开 i2c7

2. 断电关机

3. 接线方式

将 gpio shield 连接到 ROCK 4 的 40 Pin 上

4. 重启，检查 gpio shield 是否正常被识别

5. 运行测试程序

```bash
sudo python3 gpio_shield_pcf8591.py
```

该程序将读到 gpio shield 上的模拟信号。
