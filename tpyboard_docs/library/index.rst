MicroPython类库
=====================

本章介绍了构成micropython的模块（函数和类库）。有几类模块:

* 内置模块：标准Python功能的子集，用户不能扩展。
* 扩展模块：实现了Python功能的一个子集，并提供用户扩展（通过Python代码）。
* 扩展模块：实现micropython的Python标准库。
* 硬件驱动模块:特定端口或者硬件驱动的模块，因此不可移植。

Note about the availability of modules and their contents: This documentation
in general aspires to describe all modules and functions/classes which are
implemented in MicroPython. However, MicroPython is highly configurable, and
each port to a particular board/embedded system makes available only a subset
of MicroPython libraries. For officially supported ports, there is an effort
to either filter out non-applicable items, or mark individual descriptions
with "Availability:" clauses describing which ports provide a given feature.
With that in mind, please still be warned that some functions/classes
in a module (or even the entire module) described in this documentation may be
unavailable in a particular build of MicroPython on a particular board. The
best place to find general information of the availability/non-availability
of a particular feature is the "General Information" section which contains
information pertaining to a specific port.

Beyond the built-in libraries described in this documentation, many more
modules from the Python standard library, as well as further MicroPython
extensions to it, can be found in the `micropython-lib repository
<https://github.com/micropython/micropython-lib>`_.

Python标准库和微型库
---------------------------------------------

标准的Python库被 “微型化”后，就是micropython标准库。它们仅仅提供了该模块的核心功能。一些模块没有直接使用标准的Python的名字，而是冠以"u"，例如``ujson``代替``json``。也就是说micropython标准库（=微型库），只实现了一部分模块功能。通过他们的名字不同，用户有选择的去写一个Python级模块扩展功能，也是为实现更好的兼容性。

在嵌入式平台上，可添加Python级别封装库从而实现命名兼容CPython，微模块即可调用他们的u-name，也可以调用non-u-name。根据non-u-name包路径的文件可重写。

例如，``import json``的话，首先搜索一个``json.py``文件或``json``目录进行加载。如果没有找到，它回退到加载内置``ujson``模块。

.. only:: port_unix

    .. toctree::
       :maxdepth: 1

       builtins.rst
       array.rst
       cmath.rst
       gc.rst
       math.rst
       select.rst
       sys.rst
       ubinascii.rst
       ucollections.rst
       uhashlib.rst
       uheapq.rst
       uio.rst
       ujson.rst
       uos.rst
       ure.rst
       usocket.rst
       ustruct.rst
       utime.rst
       uzlib.rst

.. only:: port_tpyboard

    .. toctree::
       :maxdepth: 1

       builtins.rst
       array.rst
       cmath.rst
       gc.rst
       math.rst
       select.rst
       sys.rst
       ubinascii.rst
       ucollections.rst
       uhashlib.rst
       uheapq.rst
       uio.rst
       ujson.rst
       uos.rst
       ure.rst
       usocket.rst
       ustruct.rst
       utime.rst
       uzlib.rst

.. only:: port_wipy

    .. toctree::
       :maxdepth: 1

       builtins.rst
       array.rst
       gc.rst
       select.rst
       sys.rst
       ubinascii.rst
       ujson.rst
       uos.rst
       ure.rst
       usocket.rst
       ussl.rst
       utime.rst

.. only:: port_esp8266

    .. toctree::
       :maxdepth: 1

       builtins.rst
       array.rst
       gc.rst
       math.rst
       sys.rst
       ubinascii.rst
       ucollections.rst
       uhashlib.rst
       uheapq.rst
       uio.rst
       ujson.rst
       uos.rst
       ure.rst
       usocket.rst
       ussl.rst
       ustruct.rst
       utime.rst
       uzlib.rst


MicroPython类库详述
------------------------------

MicroPython的特有功能如下。

.. toctree::
   :maxdepth: 1

   btree.rst
   framebuf.rst
   machine.rst
   micropython.rst
   network.rst
   uctypes.rst


.. only:: port_tpyboard

   TPYBoard类库详述
   ---------------------------------

   以下是TPYBoard的具体类库。

   .. toctree::
      :maxdepth: 2

      pyb.rst
      lcd160cr.rst

.. only:: port_wipy

   Libraries specific to the WiPy
   ---------------------------------

   The following libraries are specific to the WiPy.

   .. toctree::
      :maxdepth: 2

      wipy.rst


.. only:: port_esp8266

   Libraries specific to the ESP8266
   ---------------------------------

   The following libraries are specific to the ESP8266.

   .. toctree::
      :maxdepth: 2

      esp.rst
