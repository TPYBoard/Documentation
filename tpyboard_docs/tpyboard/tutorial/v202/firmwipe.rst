[Micropython][ESP8266]TPYBoardV202固件刷新及flash擦除
========================================================

1.实验目的
-------------------

	1. 学会TPYBoard v202开发板的固件完整性检查
	2. 学会flash的擦除方法

2.准备工作
-----------------

	·所需元器件
	TPYBoard板子一块
	数据线一条
	电脑 1台（本次实验以win7为例）
	·所需软件
	PuTTY

3. 固件完整性检查
-----------------------

	第一步：下载安装所需的软件--PuTTY
	第二步：连接TPYBoard v202开发板
	通过USB数据线将电脑和TPYBoard v202开发板连接起来，会自动安装USB转串的驱动。
	安装完毕后，查看设备管理器，是否正确创建串口。

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557520973289.png

第三步：打开PuTTY，根据下图标记的红色框进行设置。

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557553767371.png

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557752936716.png

第四步：设置完成后，单击open按钮。

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557763855186.png

第五步：按下板子上的rst，进行重置。

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557774682280.png

第六步：按下图命令操作

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557784930344.png

如果最后显示True，代表固件是完整的，否则可能存在问题。
如果显示False，最好重新刷一次固件，详细操作请参考:
http://www.tpyboard.com/support/tutorial22/269.html

4. flash的擦除方法
----------------------

	第一步：安装Python环境
	从Python的官方网站http://www.python.org下载最新的2.7版本

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557824955933.png

	第二步：安装esptool和pyserial
	在命令行里运行：pip install esptool和pip install pyserial

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489557851831033.png

	第三步：擦除flash
	运行的指令：esptool.py --port COM3 erase_flash
	注意：执行擦除的指令前，需要像烧录固件一样，让esp8266进入升级模式，
	即按住板上的flash键不放，按下rst键，等待两秒，松开rst键，再松开flash键。
	否则会出现如下的错误提示：
	A fatal error occurred: Failed to connect to ESP8226
