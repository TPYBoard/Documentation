[Micropython]TPYBoardV702开发板短信功能使用说明
==========================================================

1.TPYBordV702开发板简介
---------------------------

	目前市面上唯一支持通信定位功能的MicroPython开发板：支持Python3.0及以上版本直接运行。支持GPS+北斗双模定位、GPRS通信、短信功能、电话功能；板载温湿度、光敏、三轴加速度传感器、蜂鸣器、LCD5110显示屏。免费提供定位测试服务平台。

	TPYBord_V7.0.2开发板上板载的通信器件是由深圳合方圆公司研发发的GU620通信定位模块，此模块功能强大，同时支持GPS,GPRS,GSM,蓝牙等功能，这次就来具体的说一下发送短信的功能的使用。

	能实现短信功能后还能依托着短信的功能实现温湿度的实时获取，经纬度的实时获取（可能比较费短信费哦）

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170316/1489665683266655.png


2.初识短信发送原理
---------------------

	手机的信号频率很高，一般在900Mhz左右，靠电离层反射传播，打电话的手机信号传到最近的基站，也就是移动或者连通的信号塔，再由基站把高频信号频率降低，由基站和基站之间通信，这个信号是直线传播，遇到高的建筑物会被挡住，所以那些塔都竖的很高，传到接电话的手机附近的基站，再转成高频信号发给手机。
	短消息业务(SMS-Short Message Service）的实现原理很简单：
	第一是存储转发机制。SMS传送数据包的工作由移动网络中的短消息中心而不是终端用户来完成，如果用户不在服务区内，短消息就被存储在短消息中心，等用户出现之后，再转发给他，这是GPRS等业务所不具备的。 
	第二是传递确认机制。在电路交换数据环境中，连接是端到端的，所以用户能够知道连接是否完成，以及数据传递的情况.
	通俗点说：
	基站永远是在发射信号的，其中很重要的一部分就是广播消息，广播消息中的一类是寻呼消息（含电话号码），每个手机都在时刻监听寻呼消息，当它发现一个寻呼消息是给它的话（即有人正打它电话），它就会和基站建立连接，通过基站和打你电话的人联系。
	
.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170316/1489665696262351.png

3.了解GU620的短信发送流程
------------------------------

	通过上面的描述，我们了解到了短信发送的基本原理和流程，下面我来了解一下GU620模块在应对这些流程时需要怎么做。

	首先的我们需要把模块设置到短信发送的模式，这个通过AT指令AT+CMGF=1，来设置，这条指令是设置模块为打开短信发送格式，且以文本的格式发送。

	上面一条我们说了，把短信以文本的模式发送，但是文本有很多种格式，这里我们再执行一条指令，把文本的格式设置成我们手机能稳定且正确识别的文本格式，AT+CSCS=“GB2312”，这条指令是把短信收发的文本格式设置为简体中文。

	上面两条介绍了设置发送短信的模式和文本格式，这里说一个意外事件（在想要执行的流程之外发生的事件）发生的处理方法，要是在你正要发短信的时候，有一条新的短信进来了，那这个怎么办？要是新的短信一接收到，马上显示出来，显然不是很合理，会打断我们的流程，在这里我们使用AT+CNMI=2,1指令把接收到的新消息存储到SIM卡中，然后再给出提示，在我们想读的时候再读出来，这样比较符合常理。

	在我们设置好以上的这些基本的设置步骤后，我们需要把发短信的一个重要因素，接收方的手机号码写进模块去，我们使用指令AT+CMGS=“手机号码”，这条指令是告诉模块想要通信的目的号码。

	在发送了正确的指令和手机号后模块会有提示正确的返回值，当得到这个返回值的时候，我们就可以把我们想要发送的内容（不支持汉字）编辑进去，这样模块就会把编辑的短信内容发送给前面输入的手机号码上去。

	当发送成功后，会返回发送的内容，以及相应的提示内容。

4.设置程序流程
-----------------------

	根据以上的介绍，我们大致了解了短信发送的基本流程，那么我们疾苦依据这个基本流程来设置一下程序的流程。

	1.开发板上电，红色电源指示灯会亮起；
	2.首先定义串口4，波特率设置为115200，通信模块和32芯片是依靠串口通信的；
	3.之后我们设置两个变量Message，number来存储短信内容和接收方的手机号码；
	4.在程序的顶端，总循环的外面，使用程序把引脚Y6拉低两秒以上，Y6是连接通信模块的开关机引脚的；
	5.之后拉高Y6，延时10秒，这样是为了确保通信模块正常开机；
	6.之后我们发送AT+CMGF=1\r\n，设置模块为短信发送模式；
	7.如果模块返回正确的提示内容，我们发送AT+CSCS="GB2312"\r\n，设置文本的格式；
	8.模块返回正确提示内容后，发送AT+CNMI=2,1\r\n，设置新短息的模式；
	9.设置成后，发送AT+CMGS="'+number+'"\r\n，把手机号码发送进去；
	10.当模块返回手机号码正确的提示后，发送Message+'\r\n'，把设置好的内容发送给模块；
	11.模块返回发送的文本内容及相应的发送成功的提示后，结束程序。

5.源代码
----------------

	下面是我写的简单的一个代码，提供给大家参考。

.. code-block:: python

	import pyb
	import upcd8544
	from machine import SPI,Pin
	from pyb import UART
	from ubinascii import hexlify
	from ubinascii import *
	  
	leds = [pyb.LED(i) for i in range(1,5)]
	P,L,SHUCHU=0,0,0
	SPI = pyb.SPI(1) #DIN=>X8-MOSI/CLK=>X6-SCK
	#DIN =>SPI(1).MOSI 'X8' data flow (Master out, Slave in)
	#CLK =>SPI(1).SCK  'X6' SPI clock
	RST	   = pyb.Pin('X20')
	CE	   = pyb.Pin('X19')
	DC	   = pyb.Pin('X18')
	LIGHT  = pyb.Pin('X17')
	lcd_5110 = upcd8544.PCD8544(SPI, RST, CE, DC, LIGHT)
	count_=0
	N2 = Pin('Y3', Pin.OUT_PP)
	N1 = Pin('Y6', Pin.OUT_PP)
	N1.low()
	pyb.delay(2000)
	N1.high()
	pyb.delay(10000)
	u2 = UART(4, 115200)
	Message='TPYBoard_GPS00000000000001'#输入你想要发送的短信的内容；
	number='目的号码'#输入想要发送的手机号
	w=0
	r=0
	while r<1:
		u2.write('AT+CMGF=1\r\n')
		pyb.delay(50)
		if(u2.any()>0):
			_dataRead=u2.readall()
			print('1:',_dataRead)
			lcd_5110.lcd_write_string('Message:',0,0)
			lcd_5110.lcd_write_string(str(Message),0,2)
			if(_dataRead==b'AT+CMGF=1\r\n\r\nOK\r\n'):
				u2.write('AT+CSCS="GB2312"\r\n')
				pyb.delay(50)
				if(u2.any()>0):
					_dataRead=u2.readall()
					print('2:',_dataRead)
					if(_dataRead==b'AT+CSCS="GB2312"\r\n\r\nOK\r\n'):
						u2.write('AT+CNMI=2,1\r\n')
						pyb.delay(50)
						if(u2.any()>0):
							_dataRead=u2.readall()
							print('3:',_dataRead)
							if(_dataRead==b'AT+CNMI=2,1\r\n\r\nOK\r\n'):
								u2.write('AT+CMGS="'+number+'"\r\n')
								pyb.delay(50)
								if(u2.any()>0):
									_dataRead=u2.readall()
									print('4:',_dataRead)#b'AT+CMGF=1\r\n\r\nOK\r\n'
									if(_dataRead== b'AT+CMGS="'+number+'"\r\n\r\n> '):
										u2.write(Message+'\r\n')#短信内容
										pyb.delay(50)
										if(u2.any()>0):
											_dataRead=u2.readall()
											print('5:',_dataRead)
											print(len(_dataRead))
											w=len(_dataRead)
											_dataRead=str(_dataRead)[2:w]
											print('6:',_dataRead)
											if(_dataRead==Message):
												print('7:ok')
												lcd_5110.lcd_write_string('Has been sent',0,5)
												r=10
												
6.短信群发机制作：
---------------------

	1.短信群发机是建立在上面的程序代码的基础上的；
	2.在上面的代码中，我们是建立了一个字符变量来存储电话号码，这里我们建立一个数组来 存放电话号码；
	3.在电话号码全部录入后；
	4.我们来查询一下这个数组里面有几个电话号码（也就是告诉芯片你要给几个手机发短信）；
	5.然后我们对这个数组里面的数据进行依次调用；
	6.并对这个数据执行发送短信的流程；
	7.并获取到这是数组中的第几个数据；
	8.如果数组中的数据全部调用了一次后，结束程序的发送；

	7.短信群发机例程

	下面是短信群发机的例程，给出来参考一下。

.. code-block:: python

	import pyb
	import upcd8544
	from machine import SPI,Pin
	from pyb import UART
	from ubinascii import hexlify
	from ubinascii import *
	  
	leds = [pyb.LED(i) for i in range(1,5)]
	P,L,SHUCHU=0,0,0
	SPI = pyb.SPI(1) #DIN=>X8-MOSI/CLK=>X6-SCK
	#DIN =>SPI(1).MOSI 'X8' data flow (Master out, Slave in)
	#CLK =>SPI(1).SCK  'X6' SPI clock
	RST	   = pyb.Pin('X20')
	CE	   = pyb.Pin('X19')
	DC	   = pyb.Pin('X18')
	LIGHT  = pyb.Pin('X17')
	lcd_5110 = upcd8544.PCD8544(SPI, RST, CE, DC, LIGHT)
	count_=0
	N2 = Pin('Y3', Pin.OUT_PP)
	N1 = Pin('Y6', Pin.OUT_PP)
	N1.low()
	pyb.delay(2000)
	N1.high()
	pyb.delay(10000)
	u2 = UART(4, 115200)
	Message='TPYBoard_GPS'#输入你想要发送的短信的内容；
	number=['目的号码1','目的号码2','目的号码3']#输入想要发送的手机号
	w=0
	r=0
	E=0
	Q=0
	while r<1:
		Q=len(number)
		print(Q)
		while E<Q:
			print(number[E])
			u2.write('AT+CMGF=1\r\n')
			pyb.delay(50)
			if(u2.any()>0):
				_dataRead=u2.readall()
				print('1:',_dataRead)
				lcd_5110.lcd_write_string('Message:',0,0)
				lcd_5110.lcd_write_string(str(Message),0,2)
				if(_dataRead==b'AT+CMGF=1\r\n\r\nOK\r\n'):
					u2.write('AT+CSCS="GB2312"\r\n')
					pyb.delay(50)
					if(u2.any()>0):
						_dataRead=u2.readall()
						print('2:',_dataRead)
						if(_dataRead==b'AT+CSCS="GB2312"\r\n\r\nOK\r\n'):
							u2.write('AT+CNMI=2,1\r\n')
							pyb.delay(50)
							if(u2.any()>0):
								_dataRead=u2.readall()
								print('3:',_dataRead)
								if(_dataRead==b'AT+CNMI=2,1\r\n\r\nOK\r\n'):
									u2.write('AT+CMGS="'+number[E]+'"\r\n')
									pyb.delay(50)
									if(u2.any()>0):
										_dataRead=u2.readall()
										print('4:',_dataRead)#b'AT+CMGF=1\r\n\r\nOK\r\n'
										if(_dataRead== b'AT+CMGS="'+number[E]+'"\r\n\r\n> '):
											u2.write(Message+'\r\n')#短信内容
											pyb.delay(50)
											if(u2.any()>0):
												_dataRead=u2.readall()
												print('5:',_dataRead)
												print(len(_dataRead))
												w=len(_dataRead)
												_dataRead=str(_dataRead)[2:w]
												print('6:',_dataRead)
												if(_dataRead==Message):
													print('7:ok')
													E=E+1
													lcd_5110.lcd_write_string('OK IS:'+str(E),0,5)
													pyb.delay(24000)
		r=10
		E=0
