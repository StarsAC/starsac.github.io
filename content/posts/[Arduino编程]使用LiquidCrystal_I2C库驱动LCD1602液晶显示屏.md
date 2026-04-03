+++
date = 2024-05-13T18:59:20+08:00
title = "[Arduino编程]使用LiquidCrystal_I2C库驱动LCD1602液晶显示屏"
description = "[Arduino编程]使用LiquidCrystal_I2C库驱动LCD1602液晶显示屏"
slug = "arduino-programming-with-LCD1602"
tags = ["Arduino","Hardware"]
+++

![lcd1602](https://resources.starsac.cn/2024/10/lcd1602%20(3).png)

## 1.前言

LCD1602是一种16列2行的液晶显示屏, 可以显示数字, 拉丁字母和少量符号. 原版1602显示屏使用并行数据传输, 有8个数据引脚. 可通过增加PCF8574芯片使其支持IIC总线. 支持IIC总线的1602显示屏有四个引脚:

| LCD1602引脚           | VCC  | GND  | SDA  | SCL  |
| --------------------- | ---- | ---- | ---- | ---- |
| 对应连接的Arduino引脚 | 5V   | GND  | SDA  | SCL  |

通过`LiquidCrystal_I2C`库, 我们可以很方便地通过IIC总线驱动LCD1602显示屏. 

## 2.安装`LiquidCrystal_I2C`库

新版Arduino IDE提供了库管理器, 可以很方便地在左侧边栏的库管理搜索并添加`LiquidCrystal_I2C`库.

![添加库文件](https://resources.starsac.cn/2024/10/lcd1602%20(1).png)

也可以通过[GitHub](https://github.com/johnrickman/LiquidCrystal_I2C)页面下载`.zip`文件导入Arduino IDE进行安装.

## 3.连接硬件

按照表格依次将四根导线连接到Arduino开发板上.

![连线图](https://resources.starsac.cn/2024/10/lcd1602%20(2).png)

## 4.编写程序使用Arduino库

#### 41.包含库文件, 创建对象

在程序源代码开头, 我们需要包含`LiquidCrystal_I2C`库, 并且创建LCD对象.

```c
#include <LiquidCrystal_I2C.h>
// 这里设置LCD地址为0x27, 有16列, 2行
LiquidCrystal_I2C mylcd(0x27, 16, 2);  
```

#### 42.`void setup()`函数

在`setup()`函数部分, 我们可以进行初始化, 开启背光等操作. 

```c
void setup(){
    mylcd.init();		// 初始化LCD
    mylcd.backlight();	// 打开背光
}
```

#### 43.`void loop()`函数

```c
void loop(){
    mylcd.setCursor(0,0);			// 设置光标位置
    mylcd.print("Hello World!");	// 打印文字
    mylcd.print(analogRead(A0));	// 输出变量的值
  	mylcd.setCursor(4,1);			// 可以任意设置光标的位置
  	mylcd.print("by StarsAC");
    delay(1000);
    mylcd.clear();					// 清除显示内容
    delay(1000);
}
```

## 5.其他示例程序

#### 51.使屏幕显示从串口发来的内容

```c
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C mylcd(0x27,16,2);
void setup(){
  mylcd.init();
  mylcd.backlight();
  Serial.begin(9600);	// 打开串口通讯, 设置波特率为9600
}

void loop(){
  if (Serial.available()){	// 当有数据从串口发来时
    delay(100);
    mylcd.clear();
    while (Serial.available() > 0){	// 通过while循环读取所有串口数据
      mylcd.write(Serial.read());		// 将每个字符显示至LCD屏幕
    }
  }
}
```

