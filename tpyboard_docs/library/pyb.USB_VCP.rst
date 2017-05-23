.. currentmodule:: pyb

class USB_VCP -- USB虚拟串口
======================================

micropython上的USB兼做VCP，可以通过函数去控制VCP，和PC进行数据通信。
发送的数据会在终端上直接显示出来


构造器
------------

.. class:: pyb.USB_VCP()

   创建虚拟串口对象。


方法
-------

.. method:: USB_VCP.setinterrupt(chr)

   设置中断python运行键，默认是3（Ctrl+C）。
   -1是禁止中断功能，在需要发送原始字节时需要。

.. method:: USB_VCP.isconnected()

   如果USB连接到串口设备，返回 ``True``。

.. method:: USB_VCP.any()

   如果缓冲区有数据等待接收，返回 ``True``。

.. method:: USB_VCP.close()

   这个函数什么也不做，它的目的是为了让vcp可以做为文件来使用。

.. method:: USB_VCP.read([nbytes])

   最多读取 ``nbytes`` 字节。如果不指定 ``nbytes`` 参数，那么这个函数和readall()功能相同。

.. method:: USB_VCP.readinto(buf, [maxlen])

   读取串口数据并存放到 ``buf``。如果指定maxlen参数，那么最多读取 ``maxlen`` 个字节。

.. method:: USB_VCP.readline()

   读取整行数据。

.. method:: USB_VCP.readlines()

   读取所有数据并分行存储，返回字节对象列表。

.. method:: USB_VCP.write(buf)

   写入缓冲区数据，返回写入数据的个数。

.. method:: USB_VCP.recv(data, \*, timeout=5000)

   接收数据:
   
     - ``data`` ，可以是读取数据个数，或者是缓冲区。
     - ``timeout`` ，等待接收超时时间。

.. method:: USB_VCP.send(data, \*, timeout=5000)

   Send data over the USB VCP:
   
     - ``data`` 缓冲区或者整数。
     - ``timeout`` 发送超时时间。
   
