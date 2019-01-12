:mod:`SteperMotor` --- 四相步进电机关联功能函数
==================================================

.. module:: SteperMotor
   :synopsis: 四相步进电机关联功能函数

``stepermotor`` 模块的主要功能与函数

功能相关函数
----------------------

.. only:: port_tpyboard
    
    .. class:: stepermotor.SteperMotor(pin,speed)

    创建一个SteperMotor对象。

        - ``pin`` 驱动板IN1~IN4连接的引脚对象集合，类型list
        - ``speed`` 旋转速度，单位毫秒，最小值为2
    
    .. method:: SteperMotor.steperRun(angle)
    
    控制步进电机旋转指定的角度。
    
    - ``angle`` 旋转的角度，范围在-360~360之间（负：逆时针转动）

四相步进电机与开发板接线对应引脚：
------------------------------------

		+------------+-----------+
		| TPYBoard   | 步进电机  |
		+============+===========+
		| VIN        |   ＋      |
		+------------+-----------+
		| GND        |   －      |
		+------------+-----------+
		| X1         |   IN1     |
		+------------+-----------+
		| X2         |   IN2     |
		+------------+-----------+
		| X3         |   IN3     |
		+------------+-----------+
		| X4         |   IN4     |
		+------------+-----------+

程序示例：
----------

.. code-block:: python

  from pyb import Pin
  from stepermotor import SteperMotor

  Pin_All=[Pin(p,Pin.OUT_PP) for p in ['X1','X2','X3','X4']]

  if __name__=='__main__':
    #转速(ms) 数值越大转速越慢 最小值2ms
    sm = SteperMotor(pin = Pin_All,speed=2)
    sm.steperRun(-360)
    sm.steperRun(360)