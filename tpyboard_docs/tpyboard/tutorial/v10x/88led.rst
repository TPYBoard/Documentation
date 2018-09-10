[Micropython]TPYBoard v10x 心形8*8点阵
=========================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明

实验目的
--------------------

	- 学习在PC机系统中扩展简单I/O 接口的方法。
	- 进一步学习编制数据输出程序的设计方法。
	- 学习8*8点阵与TPYBoard的接线方法，点亮点阵。

所需元器件
----------------

	- 8*8 LED点阵屏 1个
	- TPYBoard v102板子 1块
	- micro USB数据线 1条
	- 杜邦线 若干

点亮8*8 LED点阵
-----------------------

8*8 LED点阵屏原理图：

.. image:: http://tpyboard.com/ueditor/php/upload/image/20161031/1477879225908683.png


点阵后面有两排针脚，一排以1开头，即1-8针脚，一排以9开头，即9-16针脚，上图中圆圈内的数字即为针脚的对应数字。当ROW 的针脚为高电平，COL的针脚为低电平时候，我们的LED即全部点亮。
为了方便操作行和列，我们可以将ROW的8个引脚接到我们TPYBoard v102的X1-X8，COL的8个引脚接到我们TPYBoard v102的Y1-Y8。这样我们通过控制X引脚和Y引脚的高低电平就可以控制每一个LED的亮与不亮，这样就可以设计想显示的任何字符和图形，快来试试吧。


8*8 LED点阵显示心形
-----------------------

效果图：

.. image:: http://www.micropython.net.cn/ueditor/php/upload/image/20161031/1477882469674497.jpg


我们按照上面的步骤接线完毕后，编写main.py文件，下面是显示心形图案的源代码：

.. code-block:: python

	# main.py -- put your code here!
	import	pyb
	from pyb import Pin
	x_PIN = [Pin(i, Pin.OUT_PP) for i in ['X1','X2','X3','X4','X5','X6','X7','X8']]
	y_PIN = [Pin(i, Pin.OUT_PP) for i in ['Y1','Y2','Y3','Y4','Y5','Y6','Y7','Y8']]
	hanzi=['11111111','11011101','10001000','10000000','10000000','11000001','11100011','11110111']
	def displayLED():
		flag=0
		for x_ in range(0,8):
			for b in range(0,8):
				print(b)
				if b!=flag:
					x_PIN[b].value(0)
			li_l = hanzi[x_]
			y_PIN[0].value(int(li_l[:1]))
			y_PIN[1].value(int(li_l[1:2]))
			y_PIN[2].value(int(li_l[2:3]))
			y_PIN[3].value(int(li_l[3:4]))
			y_PIN[4].value(int(li_l[4:5]))
			y_PIN[5].value(int(li_l[5:6]))
			y_PIN[6].value(int(li_l[6:7]))
			y_PIN[7].value(int(li_l[7:8]))
			x_PIN[flag].value(1)
			flag=flag+1
			pyb.delay(2)
	while 1:
		displayLED()


- `下载源码 <https://github.com/TPYBoard/developmentBoard/TPYBoard-v10x-master>`_
