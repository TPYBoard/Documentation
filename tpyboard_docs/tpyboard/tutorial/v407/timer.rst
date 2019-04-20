[Micropython]TPYBoard F407 定时器和呼吸灯（PWM）
==============================================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明

TPYBoard F407开发板上有14个定时器，编号分别为1-14，其中编号3内部系统占用，5、6、7用于伺服控制和ADC/DAC。

程序示例：

.. code-block:: python

    from pyb import Timer #导入库
    tim = pyb.Timer(4) #设置Timer编号
    tim.init(freq=1) #设置频率，freq=1大约1秒1次。
    tim.counter() # 获取计数器值
    tim.callback(lambda t:pyb.LED(4).toggle()) #注册回调函数。这里使用了lambda表达式


以上程序实现的功能是，板载的第4个蓝色LED灯会每隔1秒改变一次状态，即亮/灭一次。


接下来，我们通过使用定时器的PWM功能，来实现一个呼吸灯的效果。

1、外接一个发光二极管。将发光二极管的正极接到X12引脚上，负极接到GND引脚上。

+------------------------+---------------------+
| TPYBoard F407          |  发光二极管         |
+========================+=====================+
|  GND                   |  正极（长腿）       |
+------------------------+---------------------+
|  X12                   |  负极（短腿）       |
+------------------------+---------------------+


程序示例：

.. code-block:: python

    # main.py -- put your code here!
    from pyb import Pin
    from pyb import Timer #导入库

    #------------定时器-------------#
    tim = pyb.Timer(4) #设置Timer编号
    tim.init(freq=1) #设置频率，freq=1大约1秒1次。
    tim.callback(lambda t:pyb.LED(4).toggle()) #注册回调函数。这里使用了lambda表达式

    #------------PWM呼吸灯-------------#
    p = Pin('X12',Pin.OUT_PP) 
    tim = Timer(2, freq=1000)
    ch = tim.channel(2, Timer.PWM, pin=p)

    while True:
        for i in range(60):
            ch.pulse_width_percent(i)
            pyb.delay(20)
        for k in range(60,0,-1):
            ch.pulse_width_percent(k)
            pyb.delay(20)


**程序分析**

在 micropython中实现PWM功能是通过定时器来实现的。 其实PWM是Timer的一种工
模式，需要设置 Timer 的通道、频率、 PWM 模式以及输出引脚等。
首先，我们定义了一个支持 PWM 输出的引脚。

.. code-block:: python

    p = Pin('X12',Pin.OUT_PP)  # X12 has TIM2, CH2

X12 引脚连接的是定时器 2， 通道 2（其他引脚对应的定时器和通道，详见针脚图）。`点击查看针脚图 <http://old.tpyboard.com/document/documents/tb407/407_pin.pdf>`_

.. code-block:: python

    tim = Timer(2, freq=1000)
    ch = tim.channel(2, Timer.PWM, pin=p)
           

定时器 Timer 下的 channel()方法用于设置定时器的通道，需要传递 3 个参数：

.. code-block:: python

    timer.channel(channel, mode,pin)

 - channel：定时器通道编号
 - mode： 定时器工作模式（Timer.PWM， PWM 模式） 
 - pin：驱动的引脚

在 PWM 模式下，可以调用 pulse_width_percent()方法，设置输出的占空比。 不传递参数是返回当前的占空比。还有一个 pulse_width()方法，用于设置输出的脉冲宽度。 不传递参数是返回当前的脉冲宽度。


- `下载源码 <https://github.com/TPYBoard/TPYBoard-F407>`_ 