TPYBoard v10x 驱动LCD5110显示6x8字符
==========================================


实验目的
----------------------

  - 学习LCD5110的使用方法

  - 了解SPI接口的使用方法
  

所需器件
---------------------

  - TPYBoard v10X开发板 1块
  
  - LCD5110显示屏 1个
  
  - micro USB数据线 1条
  
  - 杜邦线 若干


LCD5110显示屏的针脚含义
---------------------------

  - **RST**:复位
  - **CE** :片选
  - **DC** :数据/指令选择
  - **DIN**:串行数据线
  - **CLK**:串行时钟线
  - **VCC**:电源输入（3.3v和5v均可）
  - **BL** :背光控制端
  - **GND**:地线

  
驱动LCD5110显示屏
------------------------------------

  TPYBoard v10x的针脚与LCD5110的针脚对应关系如下:
  
  .. image:: ../img/test_21.png
  
  当小数点在你的右下角时，上面一排5个引脚，从左至右依次为g,f,GND,a,b，下面一排五个引脚，从左至右依次为 e,d,GND,c,dp。
  如果我们想让数码管显示数字8的话，需要将a,b,c,d,e,f,g所连接的TPYBoard v10x的引脚拉高，把GND与TPYBoard v10x的GND引脚接起来就可以了。
  
  main.py 内容如下::
  
    # main.py -- put your code here!
    import pyb
    import upcd8544
    from machine import SPI,Pin
    
    def main():
        SPI    = pyb.SPI(1) #DIN=>X8-MOSI/CLK=>X6-SCK
        #DIN =>SPI(1).MOSI 'X8' data flow (Master out, Slave in)
        #CLK =>SPI(1).SCK  'X6' SPI clock
        
        RST    = pyb.Pin('Y10')
        CE     = pyb.Pin('Y11')
        DC     = pyb.Pin('Y9')
        LIGHT  = pyb.Pin('Y12')
        lcd_5110 = upcd8544.PCD8544(SPI, RST, CE, DC, LIGHT)
        
        lcd_5110.lcd_write_string('Hello Python!',0,0)
        lcd_5110.lcd_write_string('Micropython',6,1)
        lcd_5110.lcd_write_string('TPYBoard',12,2)
        lcd_5110.lcd_write_string('v102',60,3)
        lcd_5110.lcd_write_string('This is a test of LCD5110',0,4)
    if __name__ == '__main__':
        main()
    
  效果图:
  
  .. image:: ../img/test_22.png
