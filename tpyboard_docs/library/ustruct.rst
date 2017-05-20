:mod:`ustruct` -- 打包和解包原始数据类型
======================================================

.. module:: ustruct
   :synopsis: 打包和解包原始数据类型

更多内容参考 `Python struct <https://docs.python.org/3/library/struct.html>`_。

支持 size/byte 的前缀: ``@``, ``<``, ``>``, ``!``.

支持的格式代码: ``b``, ``B``, ``h``, ``H``, ``i``, ``I``, ``l``,
``L``, ``q``, ``Q``, ``s``, ``P``, ``f``, ``d`` (最后2个需要浮点库支持).

函数
---------

.. function:: calcsize(fmt)

   返回需要的字节数`fmt`。

.. function:: pack(fmt, v1, v2, ...)

   按照字符串格式`fmt` 压缩参数 `v1`, `v2`, ... 。
   返回值是参数编码后的字节对象。

.. function:: pack_into(fmt, buffer, offset, v1, v2, ...)

   按照字符串格式`fmt` 压缩参数 `v1`, `v2`, ... 到缓冲区`buffer`，开始位置是`offset`。`offset`可以是负数，从缓冲区末尾开始计数。

.. function:: unpack(fmt, data)

   按照字符串格式`fmt`解压数据`data`。
   返回值是解压后参数的元组。

.. function:: unpack_from(fmt, data, offset=0)

   从 `fmt` 的 `offset` 开始解压数据，如果 `offset` 是负数就是从缓冲区末尾开始计算。
   返回值是解压后参数元组。
