.. currentmodule:: pyb
.. _pyb.LED:

class LED -- LED发光二极管对象
=======================

LED对象控制单个LED（发光二极管）


构造器
------------

.. class:: pyb.LED(id)

   创建与给定LED相关联的LED对象:
   
     - ``id`` 是LED编号, 1-4.


方法
-------

.. method:: LED.intensity([value])

   获取或设置LED强度。强度范围在0（关闭）和255（最亮）。
   如果没有参数，返回LED强度。
   如果一个参数，设置LED强度和返回``None``。

   *注：*只有LED（3）和LED（4）可以有一个平滑变化的强度，他们使用定时器PWM来实现它。
   LED（3）采用定时器（2）和LED（4）使用定时器（3）。 
   这些定时器仅配置为PWM，如果相关LED的强度设置为1和254之间的值。否则定时器是免费的一般用途。

.. method:: LED.off()

   关闭LED。

.. method:: LED.on()

   打开LED, 最大亮度。

.. method:: LED.toggle()

   切换之间的LED（最大强度）和关闭。如果LED是在非零强度，然后它被认为是 "on" 和切换将关闭它。
