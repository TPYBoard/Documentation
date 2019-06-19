.. _TPYBoard v702-index:

TPYBoard v702 典型实例
=========================================

*TPYBoard v702开发板是在v102基础上增加了GPS/GPRS的功能，基本使用完全兼容v102的教程，所以这里，我们就直接引用了v102的教程*

**注意：**
 - v702开发板增加了板载的SHT20温湿度传感使用开发板上的I2C 1(SCL = X9, SDA = X10)。
 - v702开发板上的LCD5110的接口连接的是SPI 1。
    - DIN   = X8(MOSI)
    - CLK   = X6(SCK)
    - RST   = X20
    - CE    = X19
    - DC    = X18
    - LIGHT = X17
 - v702开发板上的GU620模块使用了UART 4((TX = X1, RX = X2)。
 - v702开发板上的光敏电阻连接的是引脚Y12。
 - v702开发板上的蜂鸣器模块连接的是引脚Y3。

新手入门
-----------

帮助初学者快速掌握TPYBoard v201开发板的使用技巧。

.. toctree::
   :maxdepth: 1
   :numbered:
        
   ../v10x/start.rst
   ../v10x/repl.rst
   ../v10x/V102 (10).rst
   ../v10x/V102 (13).rst
   ../v10x/V102 (14).rst
   ../v10x/V102 (15).rst
   ../v10x/V102 (17).rst
   ../v10x/V102 (19).rst
   ../v10x/V102 (20).rst
   ../v10x/V102 (21).rst


进阶训练
-----------

学习使用TPYBoard v702开发板进行一些基础的实例训练。

.. toctree::
   :maxdepth: 1
   :numbered:
        
   ../v10x/intro.rst
   ../v10x/metaldetector.rst
   ../v10x/nightlight.rst
   ../v10x/dht11.rst
   ../v10x/88led.rst
   ../v10x/template.rst
   ../v10x/script.rst
   ../v10x/pm2.5.rst
   ../v10x/ultrasonicranging.rst
   ../v10x/diytime.rst
   ../v10x/oled.rst
   ../v10x/1602.rst
   ../v10x/airfan.rst
   ../v10x/servo.rst

高手专场
-----------

经过前面两个阶段的练习，想必你已经成为一名TPYBoard的高手了。那就来挑战下高手专场的实例训练吧！

.. toctree::
   :maxdepth: 1
   :numbered:
   
   tem.rst
   latitude.rst
   gprs.rst
   message.rst
   tel.rst
