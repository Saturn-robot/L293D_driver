[![Project Status: Inactive - The project has reached a stable, usable state but is no longer being actively developed; support/maintenance will be provided as time allows.](http://www.repostatus.org/badges/latest/inactive.svg)](http://www.repostatus.org/#inactive)

# 概述

本驱动使用GPL协议，使用本驱动请遵守相应的[协议](https://github.com/Saturn-robot/L293D_driver/blob/master/LICENSE)。

本驱动可以对直流电动机进行简单的控制，包括速度调节、方向控制等。通过该程序，你也可以确定电动机的正负极。本驱动基于Arduino Mega 2560，如果你使用的是其他版本的arduino，请视情况做相应的修改。

# L293D

该驱动板功能强大，具有以下几种功能：
- 可以支持2个5V舵机，可以连接到arduino的高分辨率专用计时器;最多支持4个直流电机，使用独立的8位速度选择（大约0.5%的解析度）
- 最多支持2个步进电机（无极或者双极），步进电机可以是单线圈的，双线圈的，interleaved或者micro-stepping
- 路H桥：L293D芯片给每路桥提供0.6A电流（峰值1.2A），并带有热保护，4.5V到25V
- 当电压过高时，下拉电阻保证电机保持停止状态
- 大终端接线端子（10-22AWG），方便连接电线
- 带有Arduino复位按钮
- 提供2个外接电源接线端子，保证数字和逻辑电源分离
- 适配Mega,Uno

## 引脚连接

将电机驱动板L293D和Arduino板的相应接口(数字一一对应)通过杜邦线连接起来，具体针脚连接方式如下：

如果只想使用直流/步进电机应该连接以下引脚：

- 数字端口11：直流电机#1/步进#1（PWM）
- 数字端口 3：直流电机#2/步进#1（PWM）
- 数字端口 5：直流电机#3/步进#2（PWM）
- 数字端口 6：直流电机#4/步进#2（PWM）

如果要控制直流/步进电机应该增加以下引脚：

- 数字引脚4：DIR CLK触发
- 数字引脚7：DIR EN指令的允许端EN
- 数字引脚8：DIR SER
- 数字引脚12：DIR ATCH中断连接

另外，GND、5V引脚必须也要连接，否则的话就无法稳定地控制直流电动机。

具体接线图如下所示(若图片加载不出来，可在项目的circuit_diagram文件夹下查看原图)：

![circuit](Test_DC_motor\circuit_diagram\L293D_bb.png)

## 安装函数库

在使用该驱动之前，你需要安装相应的函数库。该驱动使用的是Adafruit-Motor-Shield-library函数库，在仓库的lib文件夹中有相应的压缩包，直接解压到你的arduino安装路径下的library中即可。

函数库中包括驱动直流电动机、步进电机等的函数接口，使用起来十分方便。详细使用方法可以参考[这里](http://www.arduino.cn/thread-15785-1-1.html)。

## 注意事项

- 要给L293D单独供电，不要将电源接在Arduino上；
- 给L293D通电时，电源正负极千万不要接反，否则很容易烧毁板子；

# 使用本驱动

本驱动使用方法比较简单，你可以使用它来测试你的电动机或者直接驱动你的电动机，当然你也可以在该驱动的基础上进行改写，前提是你必须遵守GPL协议。

### 速度控制

对单个电动机控制：

    ｒ　100

其中命令ｒ代表设置右前轮速度，参数100为速度大小。

下表是了列举了各命令代表的含义：

|编号| 命令 | 含义 |
| :-------------: | :-------------: | :------: |
| 0 | l       | 设置左前轮       |
| 1 | r       | 设置右前轮       |
| 2 | L       | 设置左后轮       |
| 3 | R       | 设置右后轮       |
| 4 | A       | 设置所有车轮     |

**NOTE:** 我想你大概也会猜到，与其他命令不同的是命令A的参数为4个，分别为左前轮速度、右前轮速度、左后轮速度、右后轮速度。

### 方向

只要将参数设置为负值，即可改变电动机旋转方向。

### 停止

如果你想终止电动机转动，可以使用命令`s`或者`S`：

    s 0     # 终止左前轮
    S       # 终止所有车轮

命令非常好理解，通过车轮对应的编号即可终止相应的车轮。

**NOTE:**当然，将速度设置为0也可以达到相同的效果。

具体可以查看程序中的`command.h`文件:

```
#define FRONTLEFT       'l'
#define FRONTRIGHT      'r'
#define BACKLEFT        'L'
#define BACKRIGHT       'R'
#define ALLWHEELS		    'A'
#define STOPWHEEL       's'
#define STOPWHEELS      'S'
```


<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。
