[Micropython]TPYBoard DIY电子时钟
==================================
    
版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明 
    
实验目的
----------------------

    1. 学习在PC机系统中扩展简单I/O 接口的方法。
    2. 什么是SPI接口。
    3. 学习TPYBoard I2C接口的用法。
    4. 学习LCD5110接线方法。
    5. 设定时钟并将当前时间显示在LCD5110上。
    
所需元器件
---------------------

    DS3231模块一个
    TPYBoard板子一块
    LCD5110显示屏一个
    数据线一条
    杜邦线若干
    
TPYBoard的SPI接口
-------------------------------

    LCD5110需要SPI接口与TPYBoard进行连接传输数据，SPI接口是在CPU和外围低速器件之间进行同步串行数据传输，TPYBoard有两个SPI接口，我们用的为SPI1接口。
    
.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20161126/1480150621179364.png
    
TPYBoard的I2C接口
----------------------------------
    DS3231是I2C接口通信的，它通过I2C接口与TPYboard进行数据通讯，DS3231通过这个接口与TPYBoard进行双向通讯，进行数据传输，TPYBoard有两个I2C接口，我们在这用的是I2C接口1。
    
.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20161126/1480150651145892.png 
	 
.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20161126/1480150657750799.png
    
DS3231的接线方法
---------------------------------
    DS会我们在这只用到DS3231的SCL,SDA,VCC,GND四个针脚即可设定读出当前时间，我们用的位I2C接口1，即DS3231的针脚SCL接TPYboard的针脚X9，针脚SDA接TPYBoard的针脚X10。
    
.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20161126/1480150682569300.png
    
控制5110显示屏显示6x8字符
----------------------------------------
	
    先看一下LCD5110针脚含义吧（注意：LCD5110的针脚有些不一样的）
    
    TPYBoard的针脚与5110的针脚对应关系如下：
    ------------------------------------------------------------
	
    TPYBoard  LCD5110 memo
    
    # any  Pin => RST  Reset pin (0=reset, 1=normal)
    # any  Pin => CE Chip Enable (0=listen for input, 1=ignore input)
    # any  Pin => DC Data/Command (0=commands, 1=data)
    # MOSI => DIN  data flow (Master out, Slave in)
    # SCK  => CLK  SPI clock
    # 3V3  or any Pin => VCC  3.3V logic voltage (0=off, 1=on)
    # any  Pin => LIGHT Light (0=on, 1=off)
    # GND => GND
    
    还是看不明白的话，直接上针脚编号吧
    
    TPYBoard  LCD5110 memo
    
    Y10 => RST  Reset pin (0=reset, 1=normal)
    Y11    => CE    Chip Enable (0=listen for input, 1=ignore input)
    Y9 => DC    Data/Command (0=commands, 1=data)
    X8 => DIN   data flow (Master out, Slave in)
    X6 => CLK   SPI clock
    VCC
    Y12    => LIGHT Light (0=on, 1=off)
    GND
    
.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20161126/1480150739943842.png
    
源代码
-------------------

接线ok后，并且导入font.py文件、upcd8544.py文件以及DS3231.py，编写main.py设定时间，运行main.py即可将当前时间显示在5110显示屏上。
	
	.. code-block:: python
	
    1  # main.py -- put your code here!
    2  import pyb
    3  import upcd8544
    4  from machine import SPI,Pin
    5  from DS3231 import DS3231
    6    
    7  ds=DS3231(1)
    8  ds.DATE([16,11,26])
    9  ds.TIME([16,14,6])
    10    
    11 while True:
    12     ds.TEMP()
    13     ds.DATE()
    14     SPI = pyb.SPI(1) #DIN=>X8-MOSI/CLK=>X6-SCK
    15     #DIN =>SPI(1).MOSI 'X8' data flow (Master out, Slave in)
    16     #CLK =>SPI(1).SCK  'X6' SPI clock
    17         RST    = pyb.Pin('Y10')
    18         CE     = pyb.Pin('Y11')
    19         DC     = pyb.Pin('Y9')
    20         LIGHT  = pyb.Pin('Y12')
    21     lcd_5110 = upcd8544.PCD8544(SPI, RST, CE, DC, LIGHT)
    22     lcd_5110.lcd_write_string('Date',0,0)
    23     lcd_5110.lcd_write_string(str(ds.DATE()),0,1)
    24     lcd_5110.lcd_write_string('Time',0,2)
    25     lcd_5110.lcd_write_string(str(ds.TIME()),0,3)
    26     lcd_5110.lcd_write_string('Tem',0,4 )
    27     lcd_5110.lcd_write_string(str(ds.TEMP()),0,5)
    28     pyb.delay(1000)
    
DS3231.RAR link:http://www.tpyboard.com/ueditor/php/upload/file/20170111/1484102983654809.rar
    
