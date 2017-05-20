.. _quickref:

TPYBoard快速参考手册
===============================

下面是tPYBv10x的针脚图。你也可以查看其他版本的tpyboard的针脚图:.

.. image:: http://www.micropython.net.cn/ueditor/php/upload/image/20170310/1489140924942008.png
    :alt: PYBv1.0 pinout
    :width: 700px

开发板基础控制
---------------------

参考 :mod:`pyb`. ::

    import pyb

    pyb.repl_uart(pyb.UART(1, 9600)) # duplicate REPL on UART(1)
    pyb.wfi() # pause CPU, waiting for interrupt
    pyb.freq() # get CPU and bus frequencies
    pyb.freq(60000000) # set CPU freq to 60MHz
    pyb.stop() # stop CPU, waiting for external interrupt

延时和定时
----------------

使用 :mod:`time <utime>` 模块::

    import time

    time.sleep(1)           # sleep for 1 second
    time.sleep_ms(500)      # sleep for 500 milliseconds
    time.sleep_us(10)       # sleep for 10 microseconds
    start = time.ticks_ms() # get value of millisecond counter
    delta = time.ticks_diff(time.ticks_ms(), start) # compute time difference

LED发光二极管
----

参考 :ref:`pyb.LED <pyb.LED>`. ::

    from pyb import LED

    led = LED(1) # red led
    led.toggle()
    led.on()
    led.off()

引脚和通用输入输出接口
-------------

参考 :ref:`pyb.Pin <pyb.Pin>`. ::

    from pyb import Pin

    p_out = Pin('X1', Pin.OUT_PP)
    p_out.high()
    p_out.low()

    p_in = Pin('X2', Pin.IN, Pin.PULL_UP)
    p_in.value() # get value, 0 or 1

伺服控制
-------------

参考 :ref:`pyb.Servo <pyb.Servo>`. ::

    from pyb import Servo

    s1 = Servo(1) # servo on position 1 (X1, VIN, GND)
    s1.angle(45) # move to 45 degrees
    s1.angle(-60, 1500) # move to -60 degrees in 1500ms
    s1.speed(50) # for continuous rotation servos

外部中断
-------------------

参考 :ref:`pyb.ExtInt <pyb.ExtInt>`. ::

    from pyb import Pin, ExtInt

    callback = lambda e: print("intr")
    ext = ExtInt(Pin('Y1'), ExtInt.IRQ_RISING, Pin.PULL_NONE, callback)

定时器
------

参考 :ref:`pyb.Timer <pyb.Timer>`. ::

    from pyb import Timer

    tim = Timer(1, freq=1000)
    tim.counter() # get counter value
    tim.freq(0.5) # 0.5 Hz
    tim.callback(lambda t: pyb.LED(1).toggle())

脉宽调制（PWM）
----------------------------

参考 :ref:`pyb.Pin <pyb.Pin>` 和 :ref:`pyb.Timer <pyb.Timer>`. ::

    from pyb import Pin, Timer

    p = Pin('X1') # X1 has TIM2, CH1
    tim = Timer(2, freq=1000)
    ch = tim.channel(1, Timer.PWM, pin=p)
    ch.pulse_width_percent(50)

模数转换（ADC）
----------------------------------

参考 :ref:`pyb.Pin <pyb.Pin>` 和 :ref:`pyb.ADC <pyb.ADC>`. ::

    from pyb import Pin, ADC

    adc = ADC(Pin('X19'))
    adc.read() # read value, 0-4095

数模转换（DAC）
----------------------------------

参考 :ref:`pyb.Pin <pyb.Pin>` 和 :ref:`pyb.DAC <pyb.DAC>`. ::

    from pyb import Pin, DAC

    dac = DAC(Pin('X5'))
    dac.write(120) # output between 0 and 255

UART（串行总线）
-----------------

参考 :ref:`pyb.UART <pyb.UART>`. ::

    from pyb import UART

    uart = UART(1, 9600)
    uart.write('hello')
    uart.read(5) # read up to 5 bytes

SPI总线
-------

参考 :ref:`pyb.SPI <pyb.SPI>`. ::

    from pyb import SPI

    spi = SPI(1, SPI.MASTER, baudrate=200000, polarity=1, phase=0)
    spi.send('hello')
    spi.recv(5) # receive 5 bytes on the bus
    spi.send_recv('hello') # send and receive 5 bytes

I2C总线
-------

参考 :ref:`pyb.I2C <pyb.I2C>`. ::

    from pyb import I2C

    i2c = I2C(1, I2C.MASTER, baudrate=100000)
    i2c.scan() # returns list of slave addresses
    i2c.send('hello', 0x42) # send 5 bytes to slave with address 0x42
    i2c.recv(5, 0x42) # receive 5 bytes from slave
    i2c.mem_read(2, 0x42, 0x10) # read 2 bytes from slave 0x42, slave memory 0x10
    i2c.mem_write('xy', 0x42, 0x10) # write 2 bytes to slave 0x42, slave memory 0x10
