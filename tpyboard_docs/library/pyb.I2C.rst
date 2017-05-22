.. currentmodule:: pyb
.. _pyb.I2C:

class I2C -- 2线串行通信协议
=======================================

I2C是一种两线通信设备之间的协议。在物理层包括2条线：SCL和SDA，时钟线和数据线分别。

创建I2C对象连接到一个特定的总线。他们可以在创建时初始化，或初始化后

.. only:: port_tpyboard

    例如::

        from pyb import I2C

        i2c = I2C(1)                         # create on bus 1
        i2c = I2C(1, I2C.MASTER)             # create and init as a master
        i2c.init(I2C.MASTER, baudrate=20000) # init as a master
        i2c.init(I2C.SLAVE, addr=0x42)       # init as a slave with given address
        i2c.deinit()                         # turn off the peripheral

打印I2C对象提供有关其配置的信息。

.. only:: port_tpyboard

    基本方法是发送和接收::

        i2c.send('abc')      # send 3 bytes
        i2c.send(0x42)       # send a single byte, given by the number
        data = i2c.recv(3)   # receive 3 bytes
    
    接收到位，首先创建一个ByteArray::

        data = bytearray(3)  # create a buffer
        i2c.recv(data)       # receive 3 bytes, writing them into data

    可以指定超时（毫秒)::

        i2c.send(b'123', timeout=2000)   # timeout after 2 seconds

    主机必须指定收件人地址::

        i2c.init(I2C.MASTER)
        i2c.send('123', 0x42)        # send 3 bytes to slave with address 0x42
        i2c.send(b'456', addr=0x42)  # keyword for address

    其它方法::

        i2c.is_ready(0x42)           # check if slave 0x42 is ready
        i2c.scan()                   # scan for slaves on the bus, returning
                                     #   a list of valid addresses
        i2c.mem_read(3, 0x42, 2)     # read 3 bytes from memory of slave 0x42,
                                     #   starting at address 2 in the slave
        i2c.mem_write('abc', 0x42, 2, timeout=1000) # write 'abc' (3 bytes) to memory of slave 0x42
                                                    # starting at address 2 in the slave, timeout after 1 second

构造器
------------

.. only:: port_tpyboard

    .. class:: pyb.I2C(bus, ...)

       Construct an I2C object on the given bus.  ``bus`` can be 1 or 2, 'X' or
       'Y'. With no additional parameters, the I2C object is created but not
       initialised (it has the settings from the last initialisation of
       the bus, if any).  If extra arguments are given, the bus is initialised.
       See ``init`` for parameters of initialisation.

       The physical pins of the I2C busses on Pyboards V1.0 and V1.1 are:

         - ``I2C(1)`` is on the X position: ``(SCL, SDA) = (X9, X10) = (PB6, PB7)``
         - ``I2C(2)`` is on the Y position: ``(SCL, SDA) = (Y9, Y10) = (PB10, PB11)``
       
       On the Pyboard Lite:
       
         - ``I2C(1)`` is on the X position: ``(SCL, SDA) = (X9, X10) = (PB6, PB7)``
         - ``I2C(3)`` is on the Y position: ``(SCL, SDA) = (Y9, Y10) = (PA8, PB8)``
         
       Calling the constructor with 'X' or 'Y' enables portability between Pyboard
       types.

方法
-------

.. method:: I2C.deinit()

   解除I2C定义。

.. only:: port_tpyboard

    .. method:: I2C.init(mode, \*, addr=0x12, baudrate=400000, gencall=False, dma=False)

      初始化参数:

         - ``mode`` 只能是 ``I2C.MASTER`` 或者 ``I2C.SLAVE``
         - ``addr`` 7位I2C地址(only sensible for a slave)
         - ``baudrate`` 时钟频率 (only sensible for a master)
         - ``gencall`` 通用调用模式
         - ``dma`` is whether to allow the use of DMA for the I2C transfers (note
           that DMA transfers have more precise timing but currently do not handle bus
           errors properly)

    .. method:: I2C.is_ready(addr)

       检测I2C设备是否响应，只对主模式有效。

    .. method:: I2C.mem_read(data, addr, memaddr, \*, timeout=5000, addr_size=8)

       读取数据

         - ``data`` can be an integer (number of bytes to read) or a buffer to read into
         - ``addr`` is the I2C device address
         - ``memaddr`` is the memory location within the I2C device
         - ``timeout`` is the timeout in milliseconds to wait for the read
         - ``addr_size`` selects width of memaddr: 8 or 16 bits

       返回读取数据
       只对主模式有效。

    .. method:: I2C.mem_write(data, addr, memaddr, \*, timeout=5000, addr_size=8)

       Write to the memory of an I2C device:

         - ``data`` can be an integer or a buffer to write from
         - ``addr`` is the I2C device address
         - ``memaddr`` is the memory location within the I2C device
         - ``timeout`` is the timeout in milliseconds to wait for the write
         - ``addr_size`` selects width of memaddr: 8 or 16 bits

       Returns ``None``.
       This is only valid in master mode.

    .. method:: I2C.recv(recv, addr=0x00, \*, timeout=5000)

       Receive data on the bus:

         - ``recv`` can be an integer, which is the number of bytes to receive,
           or a mutable buffer, which will be filled with received bytes
         - ``addr`` is the address to receive from (only required in master mode)
         - ``timeout`` is the timeout in milliseconds to wait for the receive
    
       Return value: if ``recv`` is an integer then a new buffer of the bytes received,
       otherwise the same buffer that was passed in to ``recv``.

    .. method:: I2C.send(send, addr=0x00, \*, timeout=5000)

       Send data on the bus:

         - ``send`` is the data to send (an integer to send, or a buffer object)
         - ``addr`` is the address to send to (only required in master mode)
         - ``timeout`` is the timeout in milliseconds to wait for the send

       Return value: ``None``.

.. method:: I2C.scan()

   Scan all I2C addresses from 0x01 to 0x7f and return a list of those that respond.
   Only valid when in master mode.

Constants
---------

.. data:: I2C.MASTER

   for initialising the bus to master mode

.. only:: port_tpyboard

    .. data:: I2C.SLAVE
    
       for initialising the bus to slave mode
