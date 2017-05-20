[Micropython]TPYBoardV10X DIY温度计
=====================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明 

1、实验目的
------------------

1. 学习在PC机系统中扩展简单I/O 接口的方法。
2. 进一步学习编制数据输出程序的设计方法。
3. 学习DS18B20的接线方法，并利用DS18B20检测当前温度
3. 学习8*8LED点阵接线方法，并将当前温度显示

2、所需元器件
------------------

TPYBoard板子一块
数据线一条
杜邦线若干
8*8LED点阵一个
DS18B20温度传感器一个

3、学习DS18B20的接线方法，检测当前温度
----------------------------------------

.. image:: http://www.micropython.net.cn/ueditor/php/upload/image/20160815/1471247227119424.png


先看一下DS18B20针脚含义,如上图：

TPYBoard的针脚与DS18B20的针脚对应关系如下：

+------------+---------+
| TPYBoard   | DS18B20 |
+============+=========+
| 3V3/Pin    | VDD     |
+------------+---------+
| Pin        | DO      |
+------------+---------+
| GND        | GND     |
+------------+---------+

还是看不明白? 直接上针脚编号

+------------+---------+
| TPYBoard   | LCD5110 |
+============+=========+
| 3.3v       | VDD     |
+------------+---------+
| GND        | GND     |
+------------+---------+
| Y10        | DO      |
+------------+---------+

接线ok后，在MicroPython的源码目录中，进入drivers\onewire\目录，然后将目录下的文件ds18x20.py和onewire.py复制到PYBFLASH磁盘的根目录。复制文件后要安全退出磁盘，然后重新接入，不然找不到文件，即可运行main.py文件了，打印温度，即可用Putty看到当前的温度。

main.py源代码：
	
.. code-block:: python

	#main.py
	import pyb
	from pyb import Pin
	from ds18x20 import DS18X20
	Pin("Y11",Pin.OUT_PP).low()#GND
	Pin("Y9",Pin.OUT_PP).high()#VCC
	pyb.delay(100)
	DQ=DS18X20(Pin('Y10'))#DQ
	while True:
		tem = DQ.read_temp()
		print(tem)
		pyb.delay(1000)
			
4、点亮8*8LED点阵
----------------------

.. image:: http://www.micropython.net.cn/ueditor/php/upload/image/20160815/1471247464571951.png

	点阵后面有两排针脚，一排以1开头，即1-8针脚，一排以9开头，即9-16针脚，上图中〇中的数字对应的即为针脚的对应的数字，当ROW 的PIN NO.高电平，COL PIN NO.低电平时候，我们的LED即全部点亮了，为了方便操作行和列，我们可以将ROW的8个引脚接到我们TPYBoard的X1-X8，COL的8个引脚接到我们TPYBoard的Y1-Y8，这样我们控制X引脚Y引脚就可以控制哪个点亮跟灭了，想显示任何字都没问题，快来试试吧。

5、点将温度显示在8*8LED点阵上
--------------------------------

	接线成功以后，我们将测试出温度通过分割函数将十位，个位，小数点，以及后面的数字显示出来，代码如下：

.. code-block:: python

	import pyb
	from pyb import Pin
	from ds18x20 import DS18X20
	x_PIN = [Pin(i, Pin.OUT_PP) for i in ['X1','X2','X3','X4','X5','X6','X7','X8']]
	y_PIN = [Pin(i, Pin.OUT_PP) for i in ['Y1','Y2','Y3','Y4','Y5','Y6','Y7','Y8']]
	temp=['0000,0110,0110,0110,0110,0110,0110,0000','1101,1101,1101,1101,1101,1101,1101,1101,
	'0000,1110,1110,0000,0111,0111,0111,0000','0000,1110,1110,0000,1110,1110,1110,0000',
	'0101,0101,0101,0000,1101,1101,1101,1101','0000,0111,0111,0000,1110,1110,1110,0000',
	'0000,0111,0111,0000,0110,0110,0110,0000','0000,1110,1110,1110,1110,1110,1110,1110',
	'0000,0110,0110,0000,0110,0110,0110,0000','0000,0110,0110,0000,1110,1110,1110,0000']
	tempValue=0
	def show(l_num,r_num):
		flag=0
		for x_ in range(0,8):
			for x_ in range(0,8):
				if x_!=flag:
					x_PIN[x_].value(0)
			left_ = temp[l_num]
			left_item=left_.split(',')
			right_ = temp[r_num]
			right_item=right_.split(',')
			li_l=left_item[flag]
			li_r=right_item[flag]
			y_PIN[0].value(int(li_l[:1]))
			y_PIN[1].value(int(li_l[1:2]))
			y_PIN[2].value(int(li_l[2:3]))
			y_PIN[3].value(int(li_l[3:4]))
			y_PIN[4].value(int(li_r[:1]))
			y_PIN[5].value(int(li_r[1:2]))
			y_PIN[6].value(int(li_r[2:3]))
			y_PIN[7].value(int(li_r[3:4]))
			x_PIN[flag].value(1)
			flag=flag+1
			pyb.delay(2)
	def display(time_,l_num,r_num):
		for x in range(0,time_):
			for y in range(0,110):
				show(l_num,r_num)
	if __name__=='__main__':
		#time_t=Timer(4,freq=5,callback=randSensor)
		DQ=DS18X20(Pin('Y10'))#DQ
		while 1:
			tempValue =int(DQ.read_temp())
			print(tempValue)
			l_n=tempValue//10
			r_n=tempValue%10
			print(l_n)
			print(r_n)
			display(60,l_n,r_n)
			for i in x_PIN:
				i.value(0)
