.. currentmodule:: pyb

class Accel -- 加速度控制
====================================

控制加速度对象  使用事例::

    accel = pyb.Accel()
    for i in range(10):
        print(accel.x(), accel.y(), accel.z())

原始值范围-32到31.


构造器
------------

.. class:: pyb.Accel()

   创建并返回加速计对象。
   
方法
-------

.. method:: Accel.filtered_xyz()

   获取3维的x，y和z的值。

   实施说明：此方法当前执行为取4个样本的总和，从当前调用的前3个调用中采样，以及当前调用的示例。 因此，返回4次平均值。

.. method:: Accel.tilt()

   获取倾斜度。

.. method:: Accel.x()

   获取X轴值。

.. method:: Accel.y()

   获取Y轴值。

.. method:: Accel.z()

   获取Z轴值。

硬件说明
-------------

加速度计使用I2C总线1与处理器通信。因此，针脚X9和X10不能使用的（除了I2C）。
其他设备使用这些引脚，因此不能同时使用，是UART 1和定时器4通道1和2。