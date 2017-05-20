.. currentmodule:: pyb

class Switch -- 按键对象
=============================

按键对象是一个可定义使用的用户按钮。

用法::

     sw = pyb.Switch()       # create a switch object
     sw()                    # get state (True if pressed, False otherwise)
     sw.callback(f)          # register a callback to be called when the
                             #   switch is pressed down
     sw.callback(None)       # remove the callback

例子::

     pyb.Switch().callback(lambda: pyb.LED(1).toggle())


构造器
------------

.. class:: pyb.Switch()

   创建并返回一个按钮。


方法
-------

.. method:: Switch.__call__()

   调用按键对象来直接获取状态: ``True`` 按键按下,
   ``False`` 按键松开

.. method:: Switch.callback(fun)

   当按钮被按下时，注册回调函数。
   如果 ``fun`` 是 ``None``, 禁用回调。
