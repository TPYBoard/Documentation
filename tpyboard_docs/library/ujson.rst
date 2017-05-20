:mod:`ujson` -- JSON编码与解码
==========================================

.. module:: ujson
   :synopsis: JSON编码与解码

提供 Python 对象到 JSON（JavaScript Object Notation） 数据格式的转换。

函数
---------

.. function:: dumps(obj)

   返回 ``obj`` JSON字符串。

.. function:: loads(str)

   解析 ``str`` 字符串并返回对象。如果字符串格式错误将引发 ValueError 异常。
