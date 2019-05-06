[Micropython]TPYBoard v202 GPIO的使用
====================================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明

实验目的
--------------

- 学习在PC机系统中GPIO（General Purpose I/O Ports，即通用输入/输出端口）的使用方法
- 学会用TPYBoard接收GPIO的高低电平，控制板子上LED灯的发光与否

准备工作
----------------

- TPYBoard v202开发板 1块
- micro USB数据线 1条
- 电脑 1台（本次实验以win7为例）
- 所需软件 MicroPython File Uploader

GPIO的介绍
----------------------

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170427/1493271806734258.png

GPIO（General Purpose I/O Ports）意思为通用输入/输出端口，通俗地说，就是一些引脚，可以通过它们输出高低电平或者通过它们读入引脚的状态-是高电平或是低电平。

上图标记1为一个小的LED灯，和板子上的G2接口相连，低电平时亮，高电平灭。通过GPIO输出高低电平，从而控制LED灯的发光与否。

实验方法
-----------------

第一步：下载安装所需的软件--MicroPython File Uploader 工具（以下简称MFU）

`MFU下载 <http://tpyboard.com/download/tool/170.html>`_

第二步：连接TPYBoard v202开发板

通过USB数据线将电脑和TPYBoard v202开发板连接起来，查看设备管理器，是否正确创建端口号。打开MFU工具，选择对应的端口号，点击Open。

第三步：下载程序

在本地新建一个main.py文件，在main.py文件中输入以下源代码的内容，使用MFU工具将程序下载到TPYBoard v202开发板中。
``下载前，请先停止运行程序。``

源代码
----------------

.. code-block:: python

	from machine import Pin
	import time

	p2 = Pin(2, Pin.OUT)    # 创建一个引脚对象，使用GPIO2（G2）引脚，输出模式
	p2.value(1)             # 设置引脚输出高电平，即板载蓝色LED熄灭

	while True:
	  p2.value(0)           # 设置引脚输出低电平，即板载蓝色LED点亮
	  print(p2.value())     # 读取引脚的电平值，并打印
	  time.sleep(3)         # 延时3秒
	  p2.value(1)
	  print(p2.value())
	  time.sleep(3)

第四步：运行程序查看效果

点击MFU工具的Run/Reset，重新运行程序，或者按下板载的RST按键都可以。运行新程序后，你会看到板载的蓝色LED灯会每隔3秒亮灭一次，并一直循环下去。


- `下载源码 <https://github.com/TPYBoard/TPYBoard-v202>`_
