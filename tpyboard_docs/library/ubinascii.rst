:mod:`ubinascii` -- 二进制/ ASCII转换
============================================

.. module:: ubinascii
   :synopsis: 二进制/ ASCII转换

实现了二进制数据以ASCII形式的各种编码之间的转换（两个方向）。

函数
---------

.. function:: hexlify(data, [sep])

   将二进制数据转换为十六进制表示。

   .. admonition:: Difference to CPython
      :class: attention

      If additional argument, `sep` is supplied, it is used as a separator
      between hexadecimal values.

.. function:: unhexlify(data)

   将十六进制数据转换为二进制表示。返回字节串
   (换言之， 反二进制转换)

.. function:: a2b_base64(data)

   Base64编码的数据转换为二进制表示。返回字节串。

.. function:: b2a_base64(data)

   编码base64格式的二进制数据。返回的字符串。
