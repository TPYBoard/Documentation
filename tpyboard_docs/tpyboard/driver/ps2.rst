:mod:`PS2` --- PS2无线手柄关联功能函数
=============================================

.. module:: PS2
   :synopsis: PS2无线手柄关联功能函数

``ps2`` 模块的主要功能与函数

功能相关函数
----------------------

.. only:: port_tpyboard

    .. class:: ps2.PS2KEY(_di,_do,_cs,_clk)
 
    创建一个PS2KEY对象。
        
        - ``_di``  手柄接收器DAT连接的引脚编号
        - ``_do``  手柄接收器CMD连接的引脚编号
        - ``_cs``  手柄接收器CS连接的引脚编号
        - ``_clk`` 手柄接收器CLK连接的引脚编号

    .. method:: PS2KEY.ps2_key()

    读取手柄状态，如果有按键按下返回键值，否则返回0


PS2无线手柄与开发板接线对应引脚：
----------------------------------

		+------------+---------+
		| TPYBoard   | PS2手柄 |
		+============+=========+
		| 3V3        | VCC     |
		+------------+---------+
		| GND        | GND     |
		+------------+---------+
		| X18        | DAT     |
		+------------+---------+
		| X19        | CMD     |
		+------------+---------+
		| X20        | CS      |
		+------------+---------+
		| X21        | CLK     |
		+------------+---------+

程序示例：
----------

.. code-block:: python

  # main.py -- put your code here!
  import ps2
  
  while True:
  	ps=ps2.PS2KEY('X18','X19','X20','X21')
  	a=ps.ps2_key()	#读取手柄键值
  	if(a==13):		#判断按键执行相关灯的开关
  		pyb.LED(1).on()
  	elif(a==14):
  		pyb.LED(2).on()
  	elif(a==15):
  		pyb.LED(3).on()
  	elif(a==16):
  		pyb.LED(4).on()
  	elif(a==5):
  		pyb.LED(1).off()
  	elif(a==6):
  		pyb.LED(2).off()
  	elif(a==7):
  		pyb.LED(3).off()
  	elif(a==8):
  		pyb.LED(4).off()
        
- `点击查看源码(GitHub) <https://github.com/TPYBoard/TPYBoard_lib/>`_