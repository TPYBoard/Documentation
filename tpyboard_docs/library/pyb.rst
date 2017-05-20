:mod:`pyb` --- 开发板关联功能函数
=============================================

.. module:: pyb
   :synopsis: 开发板关联功能函数

``pyb`` 模块的主要功能与函数

时间相关函数
----------------------

.. function:: delay(ms)

   延时函数，参数的单位为毫秒。

.. function:: udelay(us)

   延时函数，参数的单位为微秒。

.. function:: millis()

   返回自上次重置电路以来的毫秒数，
   结果是一个31位有符号整数，在2**30（约12.4天）后开始返回负数。

   Note that if :meth:`pyb.stop()` is issued the hardware counter supporting this
   function will pause for the duration of the "sleeping" state. This
   will affect the outcome of :meth:`pyb.elapsed_millis()`.

.. function:: micros()

   返回自上次重置电路以来的微秒数。结果是一个31位有符号整数，在2**30微秒（约17.8分钟）后开始返回负数。

   Note that if :meth:`pyb.stop()` is issued the hardware counter supporting this
   function will pause for the duration of the "sleeping" state. This
   will affect the outcome of :meth:`pyb.elapsed_micros()`.

.. function:: elapsed_millis(start)

   返回``start``已经过去的毫秒数。
   
   负责计数器换行，并且始终返回正数。这意味着它可以用于测量高达约12.4天的周期。
   
      例如::

       start = pyb.millis()
       while pyb.elapsed_millis(start) < 1000:
           # Perform some operation

.. function:: elapsed_micros(start)

   返回已经过去的微秒数
   负责计数器换行，并且始终返回正数。这意味着它可以用于测量高达约17.8分钟的时间段。
   
   例如::

       start = pyb.micros()
       while pyb.elapsed_micros(start) < 1000:
           # Perform some operation
           pass

重置相关函数
-----------------------

.. function:: hard_reset()

   重置开发板，相当于开发板上的复位键

.. function:: bootloader()

   启动Bootloader而不使用BOOT *引脚。

.. function:: fault_debug(value)

   启用或禁用硬故障调试。硬盘故障是在底层系统发生致命错误时，如无效内存访问。

   如果值参数为`False`，那么如果存在硬故障，则主板将自动重置。

   如果值参数为`True`，当主板有硬件故障，它会打印的寄存器和堆栈跟踪，并无限期地然后循环的指示灯。

   默认值被禁用，即自动复位。
   

中断相关函数
---------------------------

.. function:: disable_irq()

   禁用中断请求。
   返回上一个IRQ状态： ``False`` / ``True`` 分别为禁用/启用的IRQ。该返回值可以传递给enable_irq将IRQ恢复到原始状态。

.. function:: enable_irq(state=True)

   启用中断请求
   如果 ``state`` 是 ``True`` (默认值) 启用中断状态。
   如果 ``state`` 是 ``False`` 禁用中断状态。
   这个函数的最常见的用法是传递返回的值``disable_irq``来退出临界区。

电源相关功能
-----------------------

.. only:: port_pyboard

    .. function:: freq([sysclk[, hclk[, pclk1[, pclk2]]]])
    
       如果没有参数，返回一个时钟频率的元组:
       (sysclk, hclk, pclk1, pclk2).
       对应如下:
    
        - sysclk: CPU频率
        - hclk: AHB总线，核心内存和DMA的频率
        - pclk1: APB1总线的频率
        - pclk2: APB2总线的频率
    
       如果给出任何参数，则函数设置CPU的频率，如果给出了其他参数，则该总线将被设置。
       频率以Hz为单位。例如频率（120000000）将sysclk（CPU频率）设置为120MHz。
       请注意，不支持所有值，并且将选择不大于给定值的最大支持频率。
    
       支持的sysclk频率为（MHz）：8,16,24,30,32,36,40,42,48,54,56,60,64,72,84,96,108,120,144,168。
       
       hclk的最大频率为168MHz，pclk1为42MHz，pclk2为84MHz。确保不要将频率设置在这些值以上。
    
       hclk，pclk1和pclk2频率使用预分频器（分频器）从sysclk频率派生。
       支持的预分频器有：1，2，4，8，16，64，128，256，512。
       支持的pclk1和pclk2的预分频器是：1,2,4,8。预分频器将被??选择为最适合所请求的频率。
    
       8MHz的sysclk频率直接使用HSE（外部晶振），16MHz直接使用HSI（内部振荡器）。
       较高的频率使用HSE驱动PLL（锁相环），然后使用PLL的输出。
    
       注意：如果在启用USB时更改频率，则USB可能会变得不可靠。在USB外设启动之前，最好在boot.py中更改频率。
       另请注意，低于36MHz的sysclk频率不允许USB正常工作。
    
    .. function:: wfi()
    
       等待内部或外部中断。
       这将执行一个``wfi``指令，降低MCU的功耗，直到发生任何中断（无论是内部还是外部），此时执行将继续。
       请注意，系统刻度中断每毫秒（1000Hz）发生一次，因此该功能将阻塞至多1ms。
    
    .. function:: stop()
    
       将TPYBoard开发板置于"sleeping"状态。
    
       这将功耗降低到小于50 uA。要从此休眠状态唤醒需要实时时钟事件或X1（PA0 = WKUP）或X18（PC13 = TAMP1）上的外部中断。醒来后系统会进行硬复位
    
       See :meth:`rtc.wakeup` to configure a real-time-clock wakeup event.
    
    .. function:: standby()
    
       Put the pyboard into a "deep sleep" state.
    
       This reduces power consumption to less than 50 uA.  To wake from this
       sleep state requires a real-time-clock event, or an external interrupt
       on X1 (PA0=WKUP) or X18 (PC13=TAMP1).
       Upon waking the system undergoes a hard reset.
    
       See :meth:`rtc.wakeup` to configure a real-time-clock wakeup event.

其他功能
-----------------------

.. only:: port_pyboard

    .. function:: have_cdc()
    
       如果USB作为串行设备连接，则返回True，否则返回False。
    
       .. 注意:: 次方法已经用pyb.USB_VCP().isconnected()代替。
    
    .. function:: hid((buttons, x, y, z))
    
       使用4元组（或列表）并将其发送到USB主机（PC）以发出HID鼠标移动事件。
    
       .. 注意:: 次方法已经用:meth:`pyb.USB_HID.send()`代替。
    
    .. function:: info([dump_alloc_table])
    
       输出开发板的信息

.. function:: main(filename)

   设置boot.py完成后要运行的主脚本的文件名。如果未调用此函数，则将执行默认文件main.py。

   在boot.py中调用此函数。

.. only:: port_pyboard

    .. function:: mount(device, mountpoint, \*, readonly=False, mkfs=False)
    
       安装一个块设备并使其作为文件系统的一部分可用。 ``device``必须是提供块协议的对象:
        - ``readblocks(self, blocknum, buf)``
        - ``writeblocks(self, blocknum, buf)`` (可选)
        - ``count(self)``
        - ``sync(self)`` (可选)
    
       ``readblocks`` 与 ``writeblocks``应该``buf``在块设备之间复制数据, 从``blocknum``设备上的块号开始。
       ``buf``是一个长度为512的倍数的字节数。
       如果``writeblocks``未定义，则设备将以 只读方式安装。这两个函数的返回值被忽略。
       ``count`` 应该返回设备上可用的块数。
       ``sync``, 如果实现，应同步设备上的数据。
    
       参数``mountpoint``是安装设备的文件系统根目录中的位置。它必须以前斜杠开头。
       如果``readonly``是``True``，那么设备将被安装为只读，否则它被安装为读写。    
       如果``mkfs``是``True``，则创建一个新的文件系统（如果尚不存在）。
       要卸载设备，请将``None``设备和安装位置作为传递``mountpoint``。

.. function:: repl_uart(uart)

   获取或设置REPL重复的UART对象。

.. only:: port_pyboard

    .. function:: rng()
    
       返回一个30位硬件产生的真随机数。

.. function:: sync()

   同步所有文件系统。

.. only:: port_pyboard

    .. function:: unique_id()
    
       返回一个12字节（96位）的字符串，这是MCU的唯一ID。

.. function:: usb_mode([modestr], vid=0xf055, pid=0x9801, hid=pyb.hid_mouse)

   如果没有参数调用，则返回当前的USB模式作为字符串。
   如果被``modestr``提供，则尝试设置USB模式。这只有在从调用``boot.py``之前调用时才能:meth:`pyb.main()`完成。
   以下值``modestr``被理解为：

   - ``None``：禁用USB
   - ``'VCP'``：使用VCP（虚拟COM端口）接口
   - ``'VCP+MSC'``：使能VCP和MSC（大容量存储设备类）
   - ``'VCP+HID'``：使能VCP和HID（人机界面设备）
   为了向后兼容，``'CDC'``被理解为是 ``'VCP'``（并且对于``'CDC+MSC'``和``'CDC+HID'``）。

   该参数``vid``和``pid``参数允许您指定VID（供应商ID）和PID（产品ID）。

   如果启用HID模式，您还可以传递``hid``关键字参数来指定HID详细信息。它需要一个元组（子类，协议，最大包长度，轮询间隔，报告描述符）。
   默认情况下，它将为USB鼠标设置适当的值。还有一个``pyb.hid_keyboard``常数，这是USB键盘的一个适当的元组。

类
-------

.. only:: port_tpyboard

    .. toctree::
       :maxdepth: 1
    
       pyb.Accel.rst
       pyb.ADC.rst
       pyb.CAN.rst
       pyb.DAC.rst
       pyb.ExtInt.rst
       pyb.I2C.rst
       pyb.LCD.rst
       pyb.LED.rst
       pyb.Pin.rst
       pyb.RTC.rst
       pyb.Servo.rst
       pyb.SPI.rst
       pyb.Switch.rst
       pyb.Timer.rst
       pyb.UART.rst
       pyb.USB_HID.rst
       pyb.USB_VCP.rst
