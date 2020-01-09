[Micropython]TPYBoard F407 超声波测距
======================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明

实验目的
--------------

- 学习在PC机系统中扩展简单I/O 接口的方法。
- 进一步学习编制数据输出程序的设计方法。
- 学习超声波模块的测距原理。
- 学习LCD5110接线方法
- 学习TPYBoard F407 Micropython开发板控制超声波模块测距。

所需元器件
--------------

- 超声波模块一个
- TPYBoard F407 Micropython开发板一块
- LCD5110显示屏一个
- 数据线一条
- 杜邦线若干
 

超声波模块工作原理 
---------------------

(1)采用IO口TRIG触发测距，给最少10us的高电平信呈。		
(2)模块自动发送 8 个 40khz 的方波，自动检测是否有信号返回。
(3)有信号返回，通过 IO 口 ECHO 输出一个高电平，高电平持续的时间就是超声波从发射到返回的时间。测试距离=(高电平时间*声速(340M/S))/2。
如下图接线，VCC 供 5V电源， GND 为地线，TRIG 触发控制信号输入，ECHO 回响信号输出等四个接口端。
  
.. image:: http://old.tpyboard.com/document/documents/tb407/supwave_1.png
                                 
硬件的连接
--------------------------------

TPYBoard F407 Micropython开发板的针脚与LCD5110的针脚对应关系如下：

+------------------------+----------------+----------------------------------------------------+
| TPYBoard F407          |     LCD5110    |                       memo                         |
+========================+================+====================================================+
|  PB8                   |     RST        |   Reset pin (0=reset, 1=normal)                    |
+------------------------+----------------+----------------------------------------------------+
|  PB6                   |     CE         |   Chip Enable (0=listen for input, 1=ignore input) |
+------------------------+----------------+----------------------------------------------------+
|  PB7                   |     DC         |   Data/Command (0=commands, 1=data)                |
+------------------------+----------------+----------------------------------------------------+
|  PB5                   |     DIN        |   data flow (Master out, Slave in)                 |
+------------------------+----------------+----------------------------------------------------+
|  PB3                   |     CLK        |   SPI clock                                        |
+------------------------+----------------+----------------------------------------------------+
|  3V3                   |     VCC        |                                                    |
+------------------------+----------------+----------------------------------------------------+
|  PB9                   |     LIGHT      |   Light (0=on, 1=off)                              |
+------------------------+----------------+----------------------------------------------------+
|  GND                   |     GND        |                                                    |
+------------------------+----------------+----------------------------------------------------+

TPYBoard F407 Micropython开发板的针脚与超声波模块的针脚对应关系如下：

+------------------------+----------------+
| F407 Micropython开发板 |  超声波模块    |
+========================+================+
|  PE2                    |     Trig      |
+------------------------+----------------+
|  PE3                    |     Echo      |
+------------------------+----------------+
|  5V                    |     VCC        |
+------------------------+----------------+
|  GND                   |     GND        |
+------------------------+----------------+

接线OK后，并且导入font.py文件和upcd8544.py文件，编写main.py将测到的距离显示在LCD5110显示屏上，运行main.py就OK了。

 
源代码
------------

.. code-block:: python

    import pyb
    from pyb import Pin
    from pyb import Timer
    import upcd8544
    from machine import SPI,Pin
    Trig = Pin('PE2',Pin.OUT_PP)
    Echo = Pin('PE3',Pin.IN)
    num=0
    flag=0
    run=1
    def start(t):
        global flag
        global num
        if(flag==0):
            num=0
        else:
            num=num+1
    def stop(t):
        global run
        if(run==0):
            run=1
    start1=Timer(1,freq=10000,callback=start)
    stop1=Timer(4,freq=2,callback=stop)

    while True:
        if(run==1):
            SPI = pyb.SPI(1)
            RST    = pyb.Pin('PB8')
            CE     = pyb.Pin('PB6')
            DC     = pyb.Pin('PB7')
            LIGHT  = pyb.Pin('PB9')
            lcd_5110 = upcd8544.PCD8544(SPI, RST, CE, DC, LIGHT)
            Trig.value(1)
            pyb.udelay(100)
            Trig.value(0)
            while(Echo.value()==0):
                Trig.value(1)
                pyb.udelay(100)
                Trig.value(0)
                flag=0
            if(Echo.value()==1):
                flag=1
                while(Echo.value()==1):           
                    flag=1
            if(num!=0):
                #print('num:',num)
                distance=num/10000*34299/2
                print('Distance')
                print(distance,'cm')
                lcd_5110.lcd_write_string('Distance',0,0)
                lcd_5110.lcd_write_string(str(distance),0,1)
                lcd_5110.lcd_write_string('cm',58,1)
                lcd_5110.lcd_write_string('This is a test of F407',0,2)
            flag=0
            run=0

- `下载源码 <https://github.com/TPYBoard/TPYBoard-F407V20/>`_ 