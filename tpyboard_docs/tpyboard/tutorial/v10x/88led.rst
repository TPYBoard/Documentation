[Micropython]TPYBoardV10X心形8*8点阵
=========================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明 

1.实验目的
--------------------

	1. 学习在PC机系统中扩展简单I/O 接口的方法。
	2. 进一步学习编制数据输出程序的设计方法。
	3. 学习8*8点阵与TPYBoard的接线方法，点亮点阵。

2.所需元器件
----------------

	8*8点阵一个
	TPYBoard板子一块
	数据线一条
	杜邦线若干

3.点亮8*8LED点阵
-----------------------

.. image:: http://www.micropython.net.cn/ueditor/php/upload/image/20161031/1477879225908683.png


	点阵后面有两排针脚，一排以1开头，即1-8针脚，一排以9开头，即9-16针脚，上图中⑨?⑧?①⑦②⑤的数字对应的即为针脚的对应的数字，当ROW 的⑨⑧①⑦②⑤为高电平，COL PIN NO.低电平时候，我们的LED即全部点亮了，为了方便操作行和列，我们可以将ROW的8个引脚接到我们TPYBoard的X1-X8，COL的8个引脚接到我们TPYBoard的Y1-Y8，这样我们控制X引脚Y引脚就可以控制哪个点亮跟灭了，想显示任何字符都没问题，快来试试吧。

4.控制8*8点阵
-----------------------

.. image:: http://www.micropython.net.cn/ueditor/php/upload/image/20161031/1477882469674497.jpg

效果图

我们按照上面的步骤做完以后，然后通电，编写main.py文件，即可显示你想要显示的字符或者图案，下面代码是在8*8点阵上显示心形图案，具体代码如下：

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
