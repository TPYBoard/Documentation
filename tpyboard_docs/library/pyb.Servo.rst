.. currentmodule:: pyb
.. _pyb.Servo:

class Servo -- 三线伺服舵机控制模块
========================================

``Servo`` 对象用于控制标准的三线伺服舵机设备（电源地、电源正、信号）。在TPYBoard v102
开发板上有4个引脚可直接使用：X1、X2、X3和X4引脚。

程序示例::

    import pyb

    s1 = pyb.Servo(1)   # 在X1引脚上创建伺服对象
    s2 = pyb.Servo(2)   # 在X2引脚上创建伺服对象

    s1.angle(45)        # 转动到45度
    s2.angle(0)         # 转动到0度

    # 移动舵机指定角度，在1500毫秒内完成
    s1.angle(-60, 1500)
    s2.angle(30, 1500)

.. note:: 舵机的驱动是通过Timer(5)产生PWM输出来实现的。使用时需注意避免冲突。

Constructors
------------

.. class:: pyb.Servo(id)

   创建一个伺服对象。  ``id`` is 1-4, 对应的引脚为 X1~X4.


Methods
-------

.. method:: Servo.angle([angle, time=0])

   不传参数，返回当前角度。

   传递参数, 设置角度:

     - ``angle`` 指定转动的角度。
     - ``time`` 达到指定角度所需的毫秒数。若忽略，将以最快的速度。

.. method:: Servo.speed([speed, time=0])

   不传参数，返回当前转动速度。

   传递参数, 设置转动速度:

     - ``speed`` 要设置的转动速度,介于-100到100之间。
     - ``time`` 达到指定速度所需的毫秒数。若忽略，将以最短的时间。

.. method:: Servo.pulse_width([value])

   不传参数，返回原始的脉冲宽度值。

   传递参数，设置脉冲宽度值。

.. method:: Servo.calibration([pulse_min, pulse_max, pulse_centre, [pulse_angle_90, pulse_speed_100]])

   不传参数，返回当前校准的数据。数据类型为元组，内含5个元素。

   传递参数，设置定时校准:

     - ``pulse_min`` 允许的最小脉冲宽度。
     - ``pulse_max`` 允许的最大脉冲宽度。
     - ``pulse_centre`` 与中心/零位相对应的脉冲宽度。
     - ``pulse_angle_90`` 90度角的脉冲宽度。
     - ``pulse_speed_100`` 与速度100相对应的脉冲宽度。
