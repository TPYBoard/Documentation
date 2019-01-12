:mod:`AT24C0X` --- AT24C0X存储器关联功能函数
=============================================

.. module:: AT24C0X
   :synopsis: AT24C0X存储器关联功能函数

``at24c0x`` 模块的主要功能与函数

功能相关函数
----------------------

.. only:: port_tpyboard

    .. class:: at24c0x.AT24C0X(id)
 
    创建一个AT24C0X对象。
        
        - ``id`` 硬件连接的I2C接口编号
        
    .. method:: writeAT24C0x(addr,data)

       写入函数。
     
       -``addr`` 写入的地址
       -``data`` 写入的数据

    .. method:: readAT24C0x(add,bit_len)

       读取函数。
       -``addr`` 读取的地址
       -``bit_len`` 读取的字节数

AT24C0X与开发板接线对应引脚：
-------------------------------

		+------------+---------+
		| TPYBoard   | AT24C0X |
		+============+=========+
		| 3V3        | VCC     |
		+------------+---------+
		| GND        | GND     |
		+------------+---------+
		| Y9         | SCL     |
		+------------+---------+
		| Y10        | SDA     |
		+------------+---------+

程序示例：
----------

.. code-block:: python

  # main.py -- put your code here!
  import at24c0x

  def main():
	at24c=at24c0x.AT24C(1)
	at24c.writeAT24C0x(0,'qxw')
	print(at24c.readAT24C0x(0,3))
  main()