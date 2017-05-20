[Micropython][ESP8266]TPYBoard V202之定时器
================================================

1.实验目的
-------------

    1. 学习在PC机系统中通过putty使用定时器功能。

2.所需元器件
--------------

    TPYBoard-esp8266开发板一块
    数据线一条

3.实现方法
---------------

    1.增加一个执行一次的定时器

    from machine import Timer
    tim = Timer(-1)  #新建一个虚拟定时器
    tim.init(period=5000, mode=Timer.ONE_SHOT, callback=lambda t:print(1))
    此处执行时程序会等待5秒打印1，period=5000 以毫秒为单位，mode=Timer.ONE_SHOT 表示只执行一次
    callback=lambda 回调函数(period=5000, mode=Timer.ONE_SHOT,t:print(1))

    2.增加一个循环定时器

    from machine import Timer
    tim = Timer(-1)  #新建一个虚拟定时器
    tim.init(period=2000, mode=Timer.PERIODIC, callback=lambda t:print(2))
    此方法执行时系统会每隔两秒无限打印2。

4.利用定时器来获取温湿度
-----------------------------

    1.硬件实物图

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489561580775098.png

    只需要将DHT11温湿度传感器的针脚与TPYBoard V202针脚连接起来即可，本次实验我们将DHT11的VCC连接TPYBoard V202的5V,DHT11的DATA连接TPYBoard V202的G4，DHT11的GND连接TPYBoard V202的GND，连接完成后，将下面代码写进TPYBoard V202即可没过几秒读取当前温湿度的值，下面是源代码:

.. code-block:: python

	from machine import Timer
	import dht
	import machine
	def f(t):
		d=dht.DHT11(machine.Pin(4))
		d.measure()
		a=d.temperature()
		b=d.humidity()
		print('温度:',a,'°C')
		print('湿度:',b,'%')
		 
	tim = Timer(-1)  #新建一个虚拟定时器
	tim.init(period=2000, mode=Timer.PERIODIC, callback=f)
   
用串口调试工具的效果图即每2秒读取一次温湿度，并打印

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170315/1489561615972014.png

若想做一下扩展，可将温湿度显示到OLED显示屏上，这样一个小型的DIY温湿度检测仪就诞生了。
