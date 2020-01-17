:mod:`TFTLCD` --- TFT液晶屏关联功能函数
=============================================

.. module:: TFTLCD
   :synopsis: TFT液晶屏关联功能函数

``tftlcd`` 模块的主要功能与函数

功能相关函数
----------------------

.. only:: port_tpyboard

    .. class:: tftlcd.TFT(spi,cs,rst,rs,color)
    
    创建一个TFT对象。
    
        - ``spi`` pyb.SPI对象
        - ``cs`` 引脚编号
        - ``rst``  引脚编号
        - ``rs``  引脚编号
        - ``color`` 颜色值

    .. method:: TFT.clean(color)

       清屏函数，color为屏幕底色

    .. method:: TFT.writecmd(cmd)

       写指令函数,cmd为要传入的指令
     
    .. method:: TFT.writedata(data)

       写指令函数,data为要传入的数据(8bit)
     
    .. method:: TFT.address_set(x,y,w,h)

       设置显示区域函数，x,y为开始坐标，w,h为终点坐标(显示内容的尺寸)
     
    .. method:: TFT.writecolor(color)

       写颜色函数，功能为写入一个颜色，color为要写入的颜色
     
    .. method:: TFT.point(x,y,color)

       画点指令，x,y为写入坐标，color为写入点的颜色
     
    .. method:: TFT.line(xs,ys,xe,ye,color)

       画线函数，xs,ys为起点坐标，xe,ye为终点坐标，color为线的颜色
     
    .. method:: TFT.fill(xs,ys,xe,ye,color)

       画填充矩形函数，xs,ys为起点坐标，xe,ye为终点坐标，color为填充的颜色
     
    .. method:: TFT.rectangle(x1,y1,x2,y2,color)

       画空心矩形函数,xs,ys为起点坐标，xe,ye为终点坐标，color为矩形线的颜色
     
    .. method:: TFT.round(x0,y0,r,color)

       画圆函数，x0,y0为圆心坐标，r为半径，color为圆边框的颜色
     
    .. method:: TFT.data8(data,color)

       写入一字节函数，功能为写入8个点，data为写入数据，color为点的颜色
     
    .. method:: TFT.write(x,y,a,b,fist,color)

       指定位置写数据函数，x,y为起点坐标，a,b为数据尺寸，fist为字库指针，color为数据颜色
       功能为在指定位置写入指定尺寸的数据，可指定颜色
     
    .. method:: TFT.write_str(x,y,wight,high,string,color)

       显示字符串函数，x,y为起点坐标，wight,high为字符尺寸，string为显示内容，color为字符串颜色
     
    .. method:: TFT.write_pictuer(x,y,wight,high,pictuer,color)

       显示指定颜色的黑白图像，x,y为起点坐标，wight,high为图像尺寸，pictuer为图像数组，color为图像颜色
     
    .. method:: TFT.init_str(font,indexes)

       字库与索引表初始化函数，font为字符库，indexes为字符索引表
     
    .. method:: displayfile(name,x,y,width,height)

       bmp图像显示函数，name为图像文件名称，x,y为图像起点，width,height为图像尺寸
 


TFT与开发板接线对应引脚：
------------------------------------

		+------------+---------+
		| TPYBoard   | TFT     |
		+============+=========+
		| Y8         | SDA     |
		+------------+---------+
		| Y6         | SCK     |
		+------------+---------+
		| X9         | RES     |
		+------------+---------+
		| X10        | RS      |
		+------------+---------+
		| X11        | CS      |
		+------------+---------+
		| GND        | GND     |
		+------------+---------+
		| 3V3        | 3V3     |
		+------------+---------+

程序示例：
------------

.. code-block:: python

    import pyb
    import tftlcd
    import font1
    import gc
    from pyb import SPI,Pin
    spi=SPI(2)
    tft=tftlcd.TFT(spi,cs='X11',rst='X9',rs='X10',color=2000)	#初始化液晶屏
    tft.clean(2000)	#清屏
    # tft.point(10,20,100)	#画点
    # tft.line(2,3,20,40,255)	#画线
    # tft.fill(0,0,30,10,0)	#画填充矩形
    # tft.rectangle(20,20,60,60,0)	#画空心矩形
    # tft.round(50,50,10,50)	#画圆

    indexes_chinese16="液晶屏测试"	#16*16汉字索引表
    indexes_chinese12="文字测试"		#12*16汉字索引表
    indexes_roman="0123456789"		# 8*16数字索引表

    tft.init_str(font1.FONT().f16,indexes_chinese16)	#设置字库及索引表
    tft.write_str(75,10,16,16,"液晶屏",0)				#显示16*16汉字

    tft.init_str(font1.FONT().f12,indexes_chinese12)	#设置字库及索引表
    tft.write_str(86,30,16,12,"测试",255)			#显示12*16汉字

    tft.init_str(font1.FONT().fnum,indexes_roman)		#设置字库及索引表
    tft.write_str(86,50,8,16,"149",tftlcd.RED)			#显示8*16数字

    tft.write_pictuer(5,40,72,75,font1.image().pictuer,tftlcd.BRED)	#显示黑白图像

    gc.enable()	#打开自动清理内存
    gc.collect()	#手动清理内存
                
    tft.displayfile("55.bmp", 0,0,67, 75)		#显示bmp图片55



- 55.bmp
 
.. image:: 55.bmp


- `点击查看源码(GitHub) <https://github.com/TPYBoard/TPYBoard_lib/>`_