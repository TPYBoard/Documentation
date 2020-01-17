:mod:`NixieTube` --- 数码管关联功能函数
=============================================

.. module:: NixieTube
   :synopsis: 数码管关联功能函数

``digital`` 模块的主要功能与函数

功能相关函数
----------------------

.. only:: port_tpyboard

    .. class:: digital.Digital(type,pins)
 
    创建一个Digital对象。
    
        - ``type`` 数码管类型。1：共阳，2：共阴
        - ``pins`` 数码管a~g脚连接的引脚对象的集合，类型list
        
    .. method:: Digital.display(data)

       显示函数。
     
       - ``data`` 要显示的数字（0~9）

数码管与开发板接线对应引脚：
-------------------------------

共阳数码管的COM接3V3，共阴数码管的COM接GND

		+------------+---------+
		| TPYBoard   | 数码管  |
		+============+=========+
		| X1         |   a     |
		+------------+---------+
		| X2         |   b     |
		+------------+---------+
		| X3         |   c     |
		+------------+---------+
		| X4         |   d     |
		+------------+---------+
		| X5         |   e     |
		+------------+---------+
		| X6         |   f     |
		+------------+---------+
		| X7         |   g     |
		+------------+---------+
		| X8         |   h     |
		+------------+---------+
		| GND/3V3    |  COM    |
		+------------+---------+

程序示例：
----------

.. code-block:: python

    import pyb
    from pyb import Pin
    from digital import Digital

    def main():
        pins = [Pin('X' + str(p),Pin.OUT_PP) for p in range(1,9)]
        d = Digital(1,pins)
        while True:
            for i in range(10):
                d.display(i)
                pyb.delay(500)

    main()
    
    
- `点击查看源码(GitHub) <https://github.com/TPYBoard/TPYBoard_lib/>`_